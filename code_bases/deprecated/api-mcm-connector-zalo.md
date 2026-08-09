# api-mcm-connector-zalo

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-mcm-connector-zalo](https://github.com/b-platform-vn/api-mcm-connector-zalo) |
| **v3 target** | `api-service-zalo` (L2) — rename opportunistically |
| **Layer** | API |
| **Status** | active |
| **Language** | TypeScript + JavaScript + Dockerfile |
| **Package** | `@b-platform-vn/api-mcm-connector-zalo` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM **Zalo OA** connector. NestJS service that integrates the Zalo Official Account channel — receives Zalo webhooks, sends outbound messages via the Zalo OpenAPI, deduplicates by `msg_id`. Corresponds to the `zalo-integration` package in the [`app-mcm`](./app-mcm.md) monorepo (`packages/integrations/zalo-integration/`, port 3012).

## Responsibility

- Receive Zalo OA webhooks (`user_send_text`, `oa_send_*`, etc.).
- Send outbound messages via Zalo OpenAPI (`POST /v3.0/oa/message/cs`).
- Deduplicate by `msg_id`; persist to MongoDB; route to omni-channel via Redis Streams.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, mongoose, passport, platform-express, schedule, typeorm), `axios`, `dotenv`, `mongodb`, `mongoose`, `redis`, `reflect-metadata`, `tslib`, `typeorm`, `upgrade`, `uuid`.
- **B-Platform SDKs**: `@b-platform-vn/sdk-mcm-common`, `@b-platform-vn/sdk-mcm-schemas`, `@b-platform-vn/sdk-mcm-streams`.
- **External**: Zalo OpenAPI (`ZALO_OPENAPI_BASE_URL`).
- **Datastores**: MongoDB, Redis.

## Public API surface

- HTTP webhook endpoints (e.g. `/api/zalo/health_check`, Zalo webhook).
- Routes via `platform-endpoints` (`/webhook/*`, `/connect/*`).

## Manifest contract

- `nest-cli.json`, `jest.config.js`, `app.yaml`, `docker-compose.local.yml`, `Dockerfile`, `.npmrc`, `docs/`, `scripts/`, `tsconfig.*`.
- Local dev scripts: `dummy-auth-server.js` (port 3003), `dummy-zalo-sdk-server.js` (port 3004), `simulate-zalo-webhook.js`, `migrate` scripts.

## Notes

- README is **detailed and bilingual** (Vietnamese + English): documents local-dev ports, dummy servers, migration. Best-documented MCM connector.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
