# api-service-facebook

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-facebook` (planned) |
| **v3 target** | `api-service-facebook` (L2) — new repo |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Facebook Messenger connector for the MCM platform. Corresponds to `packages/integrations/fb-integration/` (port 3011) in the legacy [`app-mcm`](./deprecated/app-mcm.md) monorepo.

## Responsibility

- Receive Facebook Messenger webhooks.
- Send outbound messages via Facebook Graph API.
- Persist to MongoDB; route to [`api-service-mcm`](./api-service-mcm.md) via Redis Streams.

## Dependencies

- **Runtime**: expected NestJS + `@b-platform-vn/sdk-mcm-*`.
- **External**: Facebook Graph API.
- **Datastores**: MongoDB, Redis.
- **Database Operator (L3)**: [`dbo-worker-mongodb`](./dbo-worker-mongodb.md) (planned).

## Public API surface

- HTTP webhook endpoints.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. The reference implementation lives in `app-mcm/packages/integrations/fb-integration/` — extract when ready.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
