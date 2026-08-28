# ADR — Portal-wide server-authoritative A2UI runtime

| | |
|---|---|
| **Date** | 2026-08-15 |
| **Status** | Proposed; implementation blocked until architecture approval |
| **Owners** | B-Platform Architecture / Super App Kernel |
| **Related** | [Super App architecture](../super-app.md#server-authoritative-declarative-ui), [Portal A2UI runtime requirement](../../../../technical-requirements/portal-a2ui-runtime.md), [Login first-slice plan](../../../../technical-requirements/login-json-render-a2ui-refactoring-plan.md) |

## Context

B-Platform needs server-produced declarative UI where trusted frontend widgets render data, user intent is dispatched to the server, business logic executes through the Super App kernel, and resulting UI updates stream over SSE.

The runtime must support all Portal features rather than being owned by the first login implementation. It must survive connection loss and multi-replica deployment without persisting credentials. It must also prevent generated UI from freely composing security-sensitive workflows out of atomic controls or hiding state/business behavior inside React components.

json-render provides catalog, trusted registry, renderer, and SpecStream concepts. Official A2UI v0.9.1 provides the production surface/action wire contract. These formats are complementary but are not wire-compatible by default.

## Decision

1. **Portal-wide ownership** — A2UI is a shared B-Platform Super App runtime capability under `bof-web-bplatform/apps/portal`. Login and initialization are the first vertical slice. Installed features reuse the same session, transport, reducer, catalog, action, and isolation contracts.
2. **Protocol baseline** — official A2UI v0.9.1 is the production wire baseline. json-render SpecStream is not called A2UI. A typed schema-validated adapter connects official A2UI state to trusted json-render rendering concepts.
3. **Redis authority** — production A2UI conversation/session metadata, credential-free surface snapshots, revisions, and resumable ordered event history are stored in Redis. Process-local state is test-only and Redis outage fails closed.
4. **Secure cookie session** — first bootstrap creates Redis state, generates an opaque CSPRNG A2UI session ID, and stores the ID only in an `HttpOnly`, `Secure`, SameSite, host-only cookie. The browser cannot read or select it. The A2UI session is distinct from the authenticated Portal session.
5. **Reconnect** — the cookie identifies the Redis conversation; `Last-Event-ID` is only a surface-scoped cursor. The server resumes contiguous retained events or sends a credential-free authoritative replacement. It never replays upstream actions.
6. **Cohesive catalogs** — catalog entries are reviewable workflow widgets such as `LoginCard`, `InitializationCard`, and `ApprovalPanel`. Atomic inputs, buttons, typography, icons, and layout primitives remain `shared-ui` implementation details and cannot be independently generated.
7. **Controlled/context-free widgets** — catalog widget implementations contain no inner state, React context, renderer hooks, fetches, persistence, hidden stores, authorization logic, or business handlers. The renderer host injects controlled props and typed callbacks.
8. **State separation** — runtime APIs distinguish credential-free server surface state, browser-only local presentation state, and non-serializable local secret state. Secret input never enters Redis, A2UI models/events, SSE, snapshots, storage, telemetry, logs, traces, audit, queues, or idempotency records.
9. **Server-authoritative actions** — typed A2UI action names resolve through a static Portal server registry and the Super App kernel. UI data cannot select an import, handler, provider, capability, app ID, tenant, service route, endpoint, role, permission, or return target.
10. **Surface isolation** — each surface is bound server-side to its tenant/principal, A2UI session, owning domain, catalog, revision, status, and expiry. Cross-session and non-owner access fail closed.
11. **Authentication transition** — successful authentication establishes/rotates the authenticated Portal session, clears secret state, rotates/invalidates the anonymous A2UI boundary, and rebinds only explicitly approved credential-free continuing surfaces.

## Consequences

- A single cross-feature technical requirement defines Redis keys/lifecycle, transport, renderer state, catalog policy, actions, isolation, operations, and hostile tests.
- Feature implementation plans define only their cohesive widgets, typed actions, domain capabilities, permissions, and product-specific verification.
- `shared-ui` remains protocol-agnostic and may contain stateful accessible primitives internally only when they are not catalog widgets; catalog-facing workflow widgets themselves remain controlled/context-free.
- Redis persistence, replication, backup, observability, and idempotency paths require explicit credential-exclusion tests.
- The runtime accepts less arbitrary composition than an atom-level generative catalog in exchange for smaller attack surface, stable UX, and reviewable feature contracts.
- Production authentication remains blocked until a separate decision resolves the Portal-to-orchestrator/service-provider boundary and approves identity/session contracts. During MVP, the Portal Next.js server is the entire backend and accesses MSSQL directly through a server-only data-access layer; an ADR will reintroduce the `api-service-*` / orchestrator / `dbo-*` boundary when the platform graduates past MVP.

## Rejected alternatives

- **Login-specific A2UI runtime** — rejected because later Portal features would duplicate incompatible session, transport, and state semantics.
- **Process-local conversation state** — rejected for production because reconnect and multi-replica ownership would be unreliable and unsafe.
- **Browser-visible session ID** — rejected because it increases fixation, exfiltration, and confused-deputy risk without benefit.
- **Atomic generatable control catalog** — rejected because it permits unsafe/unreviewable workflow composition and explodes the validation surface.
- **Component-owned state/context** — rejected because it bypasses deterministic A2UI state/action flow and can leak secret or stale state.
- **Persisting credentials with short TTL** — rejected because Redis persistence, replication, backups, monitoring, and crash artifacts can outlive the key.
