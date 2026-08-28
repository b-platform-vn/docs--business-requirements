# ADR — Leverage `accesscontrol` for Portal authorization

| | |
|---|---|
| **Date** | 2026-08-15 |
| **Status** | Proposed |
| **Owners** | B-Platform Architecture |
| **Related** | [Super App architecture](../super-app.md), [Portal A2UI runtime](../../../../technical-requirements/portal-a2ui-runtime.md), [Login first-slice plan](../../../../technical-requirements/login-json-render-a2ui-refactoring-plan.md) |

## Context

The [Super App architecture](../super-app.md#permission-expression-convention) defines a permission-expression convention (`app.module.action(param:value)`) with scoped parameters, wildcard/negation patterns, AND/OR combination, server-side evaluation, and deny-by-default. It does not mandate a specific enforcement engine.

During MVP, the Portal Next.js server is the entire backend and owns identity, CRM, content, and organization handlers directly (see [portal-a2ui-runtime.md](../../../../technical-requirements/portal-a2ui-runtime.md) MVP scope). Every A2UI action handler and every route gate must enforce permissions server-side before touching MSSQL. We need a single authorization engine that:

- evaluates RBAC roles + ABAC conditions in one check;
- enforces `own` vs `any` possession with a real ownership resolver (not just attribute-set selection);
- filters response attributes so handlers return only fields the caller may see;
- serializes grants to DB rows so permissions can be persisted and reloaded;
- emits an audit event on every check (granted and denied) with a stable denial reason;
- fails closed and never lets a thrown error become an accidental allow.

[`accesscontrol`](https://github.com/onury/accesscontrol) (v3, MIT, ESM, TypeScript) provides all of the above out of the box. Its API — `grant().extend()/.deny()`, `can(role).action(resource)`, `.where()` conditions, `.require()` gates, `defineCondition` async checks, `on('access')` audit events, `getGrantsList()` serialization, and `tryCan()` fail-closed checks — maps cleanly onto the existing permission convention without inventing a bespoke engine.

## Decision

Adopt [`accesscontrol`](https://github.com/onury/accesscontrol) v3 as the single authorization engine for the Portal server during MVP. No bespoke RBAC/ABAC evaluation is written; all permission checks go through `accesscontrol`.

### 1. Mapping the B-Platform permission convention to `accesscontrol`

The existing `app.module.action(param:value)` convention maps to `accesscontrol` concepts as follows:

| Convention concept | `accesscontrol` concept |
|---|---|
| `app.module.action` | Custom action via `.action(action, resource)` / `.do(action, resource)` (e.g. `crm.communication.list_conversations`). |
| `app` / `module` | Group/category: `crm/communication` (bounded bulk grants via `setup()`). |
| `param:value` scoped parameters | `.where()` conditions over context, e.g. `$.division in ["odl","mdf"]`; scoped values become condition context, not separate roles. |
| `param:*` wildcard | Condition omitting that parameter (grant applies for any value), or `$.param != null`. |
| `param:!value` negation | `.where('$.param != "value"')` condition; or a `.deny()` carve-out. |
| Multiple values (`a,b`) = OR | `$.param in ["a","b"]` condition. |
| Multiple parameters = AND | Logical `and` in one `.where()` expression. |
| Deny always wins | `accesscontrol` deny-overrides: `.deny(role).readAny(resource)`; also `.deny()` does not cascade across possession. |

Scoped parameters are not separate roles — they are ABAC conditions evaluated at check time against the request context (tenant, division, the resource record, the authenticated principal). Roles represent job functions (root, admin, operator, viewer); conditions represent the object/environment scope.

### 2. Ownership

`accesscontrol` v3 enforces `own` with a real ownership resolver. The Portal configures `policy.owner` to resolve from the MSSQL record:

```ts
const ac = new AccessControl(grants, {
  policy: {
    owner: (ctx, { resource }) => ctx.record?.ownerId === ctx.user?.id,
    strict: { actions: true, resources: true },
  },
});
```

Handlers calling `can(role, { user, record }).updateOwn(resource)` get a real ownership verdict, not just an attribute set. A blanket `any` grant still satisfies an `own` check, per `accesscontrol` semantics.

### 3. Attribute filtering

Handlers call `perm.filter(record)` to return only fields the caller may see — e.g. `['*', '!password']` or `['*', '!secret', 'profile.*']`. This replaces ad-hoc field-stripping in handler code and is the single attribute-filtering boundary for MSSQL read results.

### 4. Serialization

Grants are persisted as MSSQL rows via `ac.getGrantsList()` (flat, DB-friendly) and restored via `new AccessControl(rows)`. The grants table is loaded at Portal startup and reloaded on role/permission mutation. `ac.snapshot()`/`ac.restore()` round-trips the whole model (grants + gates + vocabulary).

### 5. Audit

`ac.on('access', (e) => audit(e))` captures `{ roles, resource, action, granted, reason, ... }` for every check — granted and denied. Denial reasons (`require_failed`, `condition_failed`, `no_grant`, `out_of_schedule`, etc.) feed the A2UI runtime's audit record. Listeners are observational and isolated; a throwing listener never breaks a check.

### 6. Fail-closed checks

On the request path, handlers use `ac.tryCan(role, ctx).action(action, resource)` — `tryCan` never throws; a thrown error can never become an accidental allow. Errors carry a stable `err.code` for telemetry.

### 7. Gates

`.require()` mandatory gates enforce cross-cutting restrictions: `$.env == "prod"` (production-only grants), `$.mfa == true` (MFA-gated mutations), `$.ip cidr 10.0.0.0/8` (network-zone gates). Gates only restrict; `granted = (grant matches) AND (every applicable gate passes)`.

### 8. Async conditions

Custom conditions (`defineCondition`) support async business checks — e.g. IP allowlist, tenant-membership lookup against MSSQL. Synchronous declarative checks use `.can()`; async ones use `.grantedAsync` / `.checkAsync()`.

## Consequences

- The Portal server has one authorization engine, not a bespoke one. Permission definitions, gates, and conditions are declarative and serializable.
- The existing permission-expression convention (`app.module.action(param:value)`) is preserved as the human-readable contract; `accesscontrol` is the enforcement engine underneath.
- Handler code is simpler: it calls `tryCan`, checks `.granted`, and calls `.filter()` on results, rather than reimplementing role/condition/ownership logic.
- `accesscontrol`'s deny-overrides, fail-closed `tryCan`, prototype-pollution safety, ReDoS-guarded opt-in regex, and `safeErrors` are inherited without custom work.
- The grants table is a real MSSQL artifact; role/permission mutations go through `getGrantsList()`/`snapshot()` and reload.
- Every authorization decision is auditable through the `access` event stream, feeding the A2UI runtime's bounded audit records.

## Rejected alternatives

- **Casbin** — powerful but configuration-heavy; the TOML/policy-file model diverges from the B-Platform convention's `app.module.action(param:value)` syntax more sharply, and attribute filtering is less first-class.
- **Bespoke RBAC engine** — rejected because it duplicates deny-overrides, ownership resolution, condition evaluation, attribute glob filtering, serialization, and audit that `accesscontrol` already provides and mutation-tests.
- **Client-side checks only** — rejected; the Super App architecture already requires server-side evaluation, and `accesscontrol` is server-only by design.

## Dependencies

- `accesscontrol` v3 (MIT, ESM-only, TypeScript).
- The Portal server's server-only MSSQL data-access layer (for ownership resolution and grant persistence).
- The A2UI runtime action ingress (§8) — every handler calls `tryCan` before touching MSSQL.

## Open questions

- Should grants be loaded from MSSQL at startup only, or cached with a reload-on-mutation signal? (MVP: load at startup, reload on role/permission mutation.)
- Should the `access` audit listener batch writes or emit one row per check? (MVP: one row per check for correctness; batch in post-MVP if throughput requires.)
- When the platform graduates past MVP and handlers move to `api-service-*`, should `accesscontrol` live in each service, or stay in the Portal and mediate cross-service calls? (Deferred to post-MVP ADR.)
