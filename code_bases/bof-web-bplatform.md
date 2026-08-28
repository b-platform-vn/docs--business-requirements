# bof-web-bplatform

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/bof-web-bplatform` |
| **Previous name** | `web-admin-portal` → [old doc](./deprecated/web-admin-portal.md) |
| **v3 name** | `bof-web-bplatform` (L1 / bof) |
| **Layer** | L1 — User Interaction (bof) |
| **Status** | active scaffold — `apps/portal` + `packages/shared-ui` monorepo, admin shell, login UI, and portal BFF skeleton shipped; domain module packages planned |
| **Language** | TypeScript (Next.js 15 App Router, React 19, Tailwind v4) |
| **Package manager** | pnpm 9.12 workspaces |
| **Default branch** | `main` |

Last synced: 2026-08-15

## Purpose

Next.js App Router monorepo for the **B-Platform Super App backoffice** — `admin.b-platform.vn`. Hosts the operator surface for every B-Platform back-office module in a single portal shell. `apps/portal` is the single app container and domain modules are installed as packages under `packages/<module>`.

## Responsibility

This is the **single backoffice web** for all B-Platform operator modules. The portal owns routing, shell, feature toggles, and permission handling; each module package owns its routes, pages, server actions, and permission specs.

- **`apps/portal`** (shipped scaffold) — single Next.js app container. Owns App Router routing, the admin shell (`Sidebar` + `Header`), feature toggles, permission gating, login UI, and server-side BFF skeleton. The canonical remote currently forwards module calls to [`api-service-orchestrator`](./api-service-orchestrator.md); modules never hold an L2 client.
- **`packages/unigate`** (planned) — manage applications, users, roles, and permissions. Module package; operator surface for [`api-service-identity`](./api-service-identity.md).
- **`packages/crm`** (planned) — manage customers, support tickets, communications (Email, Chat), promotions/announcements. Module package; operator surface for [`api-service-crm`](./api-service-crm.md).
- **`packages/content`** (planned) — manage static pages, media (already in DB or uploaded via Chat), and files (uploaded by apps). Module package; operator surface for [`api-service-content`](./api-service-content.md).
- **`packages/shared-ui`** (shipped scaffold) — private workspace package for atomic and base components shared across all modules. The remote public source currently exports `Button` and `Card`.

## Dependencies

- **Runtime**: `next` 15.2.4, `react` / `react-dom` 19.0.0, `axios` ^1.14.0, `react-icons` ^5.6.0, workspace `@b-platform-vn/shared-ui`.
- **Dev**: Tailwind v4 (via `@tailwindcss/postcss`), ESLint 9 with `eslint-config-next`, TypeScript ^5.6, Prettier ^3.3.3.
- **Backend (L2, via orchestrator)** — the portal forwards module service calls to [`api-service-orchestrator`](./api-service-orchestrator.md); the orchestrator dispatches to the owning L2 service. Modules never hold an L2 client:
  - `packages/unigate` → [`api-service-identity`](./api-service-identity.md) (operator auth/SSO, users, roles, permissions).
  - `packages/crm` → [`api-service-crm`](./api-service-crm.md) (customers, tickets, communications, promotions).
  - `packages/content` → [`api-service-content`](./api-service-content.md) (static pages, media, files).
  - `packages/quote` → [`api-service-organization`](./api-service-organization.md) (B2B quote).
- **Shared packages**: private workspace `@b-platform-vn/shared-ui`; [`sdk-platform`](./sdk-platform.md) schemas/clients remain planned. The current BFF uses native `fetch`; `axios` is declared but not used by the forwarding skeleton.

## Public API surface

- Web UI and server-side portal BFF only — no external public package/API contract. The private workspace packages `@b-platform-vn/portal` and `@b-platform-vn/shared-ui` are consumed inside this monorepo and are not published.
- Current server surface is an internal skeleton: `src/server/orchestrator-client.ts` exports typed `forward<T>()`; `src/server/actions/auth.ts` exports the login server action. The identity/session contract is explicitly TODO and is not yet a stable public API.
- The portal-wide server-authoritative runtime is specified in [`technical-requirements/portal-a2ui-runtime.md`](../technical-requirements/portal-a2ui-runtime.md). The [login plan](../technical-requirements/login-json-render-a2ui-refactoring-plan.md) is its first vertical slice; none of the Redis session store, A2UI transport, cohesive-widget catalog/registry, or surface runtime is shipped on remote `main` yet.

## Monorepo topology

```
apps/
  portal/              # single Next.js app container
    app/               # dynamic routes matching all installed module routes
    (auth)/            # unauthenticated routes — login
    (main)/            # authenticated admin shell — mounts <AdminShell/>
    layout.tsx
    page.tsx
    server/            # BFF: forwards {package} calls → api-service-orchestrator
    features.ts        # feature toggles + permission handling
packages/
  unigate/             # routes, pages, server actions, provided/required permissions
  crm/
  content/
  quote/
  shared-ui/           # atomic + base components shared across all modules
```

Topology rules:

- **`apps/portal` is the single app.** It owns routing, shell, feature toggles, permission gating, and the BFF that forwards service calls to `api-service-orchestrator`. No module package is a standalone app.
- **Each `packages/<module>` owns** its routes, pages, server actions, and a permission spec (what permissions it provides and requires). The portal reads each package's route + permission manifest to wire dynamic routes and gates.
- **`packages/shared-ui`** holds atomic and base components shared across modules. Module packages depend on `shared-ui`, not on each other.
- **Module packages never call L2 services directly.** They hand service calls to the portal BFF, which forwards to `api-service-orchestrator`. Cross-domain calls are routed by the orchestrator, not by the portal or a module.
- **Current state** (verified against remote `main` 2026-08-12): the `apps/portal` + `packages/shared-ui` split is shipped. Domain packages remain planned.

## Notes

- ✅ v3 rename is complete: the canonical remote is `github.com/b-platform-vn/bof-web-bplatform`.
- **Shipped vs. planned** — shipped = monorepo root, portal shell/BFF skeleton, login UI, and shared-ui scaffold; planned = UniGate, CRM, Content, and Quote domain packages. Any module marked "(planned)" has no `packages/<module>` on the remote yet.
- **Remote verification 2026-08-15** — no A2UI/json-render runtime, SSE route, surface store, or action route exists on `main`; `LoginCard` remains a hardcoded client form. `packages/shared-ui` still exports only `Button` and `Card`. More complete local auth atoms and visual refinements are unmerged WIP and are not treated as canonical architecture facts.
- **Planned Portal runtime, not shipped** — all adopting Super App features will reuse one Redis-backed, cookie-bound A2UI conversation runtime. Catalogs expose cohesive controlled widgets while `shared-ui` atoms remain implementation details; login/initialization is the first planned conformance slice.
- README documents the product topology, layer boundary, local commands, package scope, and remote source-of-truth rule.
- ⚠️ Architecture drift remains between the portal repository contract and [`api-service-orchestrator`](./api-service-orchestrator.md): the portal remote currently consumes the orchestrator, while the orchestrator doc defines it as L2-internal and "not consumed by L1." Until an ADR resolves this, implementation must follow the target repository's documented/implemented server boundary and must not let declarative UI select services or endpoints.
- The operator modules map 1:1 to L2 business domains — UniGate↔identity, CRM↔crm, Content↔content, Quote↔organization — so the backoffice package list mirrors the L2 domain topology.

## Owner

- Engineering (routed via Multica). Architecture: B-Platform architect owns this doc only.
