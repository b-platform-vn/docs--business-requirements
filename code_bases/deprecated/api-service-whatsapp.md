# api-service-whatsapp

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-whatsapp` (planned) |
| **v3 target** | `api-service-whatsapp` (L2) — new repo |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

WhatsApp Business connector for the MCM platform. Corresponds to `packages/integrations/wa-integration/` (port 3013) in the legacy [`app-mcm`](./deprecated/app-mcm.md) monorepo.

## Responsibility

- Receive WhatsApp Business webhooks.
- Send outbound messages via WhatsApp Business API.
- Persist to MongoDB; route to [`api-service-mcm`](./api-service-mcm.md) via Redis Streams.

## Dependencies

- **Runtime**: expected NestJS + `@b-platform-vn/sdk-mcm-*`.
- **External**: WhatsApp Business API.
- **Datastores**: MongoDB, Redis.
- **Database Operator (L3)**: [`dbo-worker-mongodb`](./dbo-worker-mongodb.md) (planned).

## Public API surface

- HTTP webhook endpoints.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. The reference implementation lives in `app-mcm/packages/integrations/wa-integration/` — extract when ready.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
