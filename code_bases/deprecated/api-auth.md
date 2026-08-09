# api-auth

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-auth](https://github.com/b-platform-vn/api-auth) |
| **v3 target** | `api-service-auth` (L2) — rename opportunistically |
| **Layer** | API |
| **Status** | active |
| **Language** | TypeScript + Dockerfile |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Authentication service. NestJS application (confirmed via `nest-cli.json` + `src/` + `app.yaml` + `Dockerfile`). Serves the B-Platform auth surface (sign-in / SSO / token validation) — verify exact scope against `src/` before asserting further.

## Responsibility

- Authentication / token issuance / validation for B-Platform services.
- Exposes auth endpoints consumed by `platform-endpoints` webhook/API routing and by frontends via Server Actions.

## Dependencies

- **Runtime**: NestJS (per `nest-cli.json`, `app.yaml`, `Dockerfile`). Specific dep list `TBD` — `package.json` fetch returned empty against the remote (likely transient; re-verify before writing more).
- **Infra**: deployed via `app.yaml` (Cloud Run-style) + Docker.

## Public API surface

- HTTP auth endpoints (TBD specific routes — re-fetch `src/` controller list to document).

## Notes

- ⚠️ README is empty on `main`. Doc gap: open a Multica issue to add a product README describing the auth surface.
- `api-auth` is referenced by other MCM connectors' local-dev `dummy-auth-server` (port 3003, `/api/auth/validate`) — confirm whether `api-auth` itself implements that contract in production.

## Owner

- Engineering (platform/auth). Architecture: B-Platform architect owns this doc only.
