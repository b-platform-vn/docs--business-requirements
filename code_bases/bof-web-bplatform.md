# bof-web-bplatform

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/bof-web-bplatform` (planned) |
| **Current name** | `web-admin-portal` → [old doc](./deprecated/web-admin-portal.md) |
| **v3 target** | `bof-web-bplatform` (L1 / bof) — rename opportunistically |
| **Layer** | L1 — User Interaction (bof) |
| **Status** | partial — single-app scaffold + admin shell + login + quote shipped; restructure to apps/packages monorepo + UniGate/CRM/Content packages planned |
| **Language** | TypeScript (Next.js 15 App Router, React 19, Tailwind v4) |
| **Package manager** | pnpm workspaces (planned) — currently a single `web` package |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Next.js App Router monorepo for the **B-Platform Super App backoffice** — `admin.b-platform.vn`. Hosts the operator surface for every B-Platform back-office module in a single portal shell. Bootstrapped with `create-next-app`; target topology is an `apps/` + `packages/` workspace where `apps/portal` is the single app container and each back-office module is an installable package under `packages/<module>`.

## Responsibility

This is the **single backoffice web** for all B-Platform operator modules. The portal owns routing, shell, feature toggles, and permission handling; each module package owns its routes, pages, server actions, and permission specs.

- **`apps/portal`** (planned target; shell + scaffold shipped) — single Next.js app container. Owns dynamic routing to match all installed module routes, the admin shell (`Sidebar` + `Header`), feature toggles, and permission gating. Forwards every service call from a module package to [`api-service-orchestrator`](./api-service-orchestrator.md) — modules never call an L2 service directly, the portal is the single BFF.
- **`packages/unigate`** (planned) — manage applications, users, roles, and permissions. Module package; operator surface for [`api-service-identity`](./api-service-identity.md).
- **`packages/crm`** (planned) — manage customers, support tickets, communications (Email, Chat), promotions/announcements. Module package; operator surface for [`api-service-crm`](./api-service-crm.md).
- **`packages/content`** (planned) — manage static pages, media (already in DB or uploaded via Chat), and files (uploaded by apps). Module package; operator surface for [`api-service-content`](./api-service-content.md).
- **`packages/shared-ui`** (planned) — atomic and base components shared across all modules.
- **Shipped (in current single-app layout)** — admin shell (`src/components/admin-shell.tsx`), `login` feature (`src/features/login`), `quote` feature (`src/features/quote`). These migrate into the apps/packages topology.

## Dependencies

- **Runtime**: `next` 15.2.4, `react` / `react-dom` 19, `axios` ^1.14.0, `react-icons` ^5.6.0.
- **Dev**: Tailwind v4 (via `@tailwindcss/postcss`), ESLint 9 with `eslint-config-next`, TypeScript ^5 (planned).
- **Backend (L2, via orchestrator)** — the portal forwards module service calls to [`api-service-orchestrator`](./api-service-orchestrator.md); the orchestrator dispatches to the owning L2 service. Modules never hold an L2 client:
  - `packages/unigate` → [`api-service-identity`](./api-service-identity.md) (operator auth/SSO, users, roles, permissions).
  - `packages/crm` → [`api-service-crm`](./api-service-crm.md) (customers, tickets, communications, promotions).
  - `packages/content` → [`api-service-content`](./api-service-content.md) (static pages, media, files).
  - `packages/quote` → [`api-service-organization`](./api-service-organization.md) (B2B quote).
- **Shared (L0)**: [`sdk-platform`](./sdk-platform.md) for shared schemas/clients (planned; currently uses direct `axios`).

## Public API surface

- Web UI only — no public package export. Served at `admin.b-platform.vn` (with 24h WebSocket timeout for admin sessions). Module packages are consumed only by `apps/portal`, not published.

## Monorepo topology (target)

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
- **Current state** (verified against remote `main` 2026-08-09): still a single `web` package with `src/{app,components,features,integrations}`. The `apps/`+`packages/` split is the target; `login` and `quote` are the features to migrate first.

## Notes

- ⚠️ v3 rename (`web-admin-portal` → `bof-web-bplatform`) is **planned**, not yet applied on the remote. Current remote repo name is `web-admin-portal`.
- **Shipped vs. planned** — shipped = single-app scaffold + admin shell + login + quote; planned = apps/packages restructure + UniGate, CRM, Content packages + shared-ui. Any module marked "(planned)" has no `packages/<module>` on the remote yet.
- README is still the default `create-next-app` boilerplate; no product-specific README yet.
- The operator modules map 1:1 to L2 business domains — UniGate↔identity, CRM↔crm, Content↔content, Quote↔organization — so the backoffice package list mirrors the L2 domain topology.

## Owner

- Engineering (routed via Multica). Architecture: B-Platform architect owns this doc only.
