# api-service-zalo

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-zalo` (planned) |
| **Current name** | `api-mcm-connector-zalo` → [old doc](./deprecated/api-mcm-connector-zalo.md) |
| **v3 target** | `api-service-zalo` (L2) — rename opportunistically |
| **Layer** | L2 — API Services |
| **Status** | planned (rename of `api-mcm-connector-zalo`) |
| **Language** | TypeScript + JavaScript + Dockerfile |
| **Package** | `@b-platform-vn/api-mcm-connector-zalo` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Zalo OA connector. NestJS service that integrates the Zalo Official Account channel — receives Zalo webhooks, sends outbound messages via the Zalo OpenAPI, deduplicates by `msg_id`. Corresponds to `packages/integrations/zalo-integration/` (port 3012) in the legacy [`app-mcm`](./deprecated/app-mcm.md) monorepo.

## Responsibility

- Receive Zalo OA webhooks (`user_send_text`, `oa_send_*`, etc.).
- Send outbound messages via Zalo OpenAPI (`POST /v3.0/oa/message/cs`).
- Deduplicate by `msg_id`; persist to MongoDB; route to omni-channel via Redis Streams.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, mongoose, passport, platform-express, schedule, typeorm), `axios`, `dotenv`, `mongodb`, `mongoose`, `redis`, `reflect-metadata`, `tslib`, `typeorm`, `uuid`.
- **B-Platform SDKs (L0)**: `@b-platform-vn/sdk-mcm-common`, `@b-platform-vn/sdk-mcm-schemas`, `@b-platform-vn/sdk-mcm-streams`.
- **External**: Zalo OpenAPI (`ZALO_OPENAPI_BASE_URL`).
- **Datastores**: MongoDB, Redis.
- **Database Operator (L3)**: [`dbo-worker-mongodb`](./dbo-worker-mongodb.md) (planned).

## Public API surface

- HTTP webhook endpoints (e.g. `/api/zalo/health_check`, Zalo webhook).
- Routes via [`platform-endpoints`](./platform-endpoints.md) (`/webhook/*`, `/connect/*`).

## Notes

- README is **detailed and bilingual** (Vietnamese + English): documents local-dev ports, dummy servers, migration. Best-documented MCM connector.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
