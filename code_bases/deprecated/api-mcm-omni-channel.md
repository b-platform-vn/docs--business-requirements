# api-mcm-omni-channel

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-mcm-omni-channel](https://github.com/b-platform-vn/api-mcm-omni-channel) |
| **v3 target** | `api-service-mcm` (L2) — fold with `api-mcm-connector-internal` into one MCM module service |
| **Layer** | API |
| **Status** | active |
| **Language** | TypeScript + JavaScript + Dockerfile |
| **Package** | `@b-platform-vn/api-mcm-omni-channel` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM **omni-channel** router. NestJS service that handles message routing / fan-out across channels. Corresponds to the `omni-channel` package in the [`app-mcm`](./app-mcm.md) monorepo (`packages/omni-channel/`, port 3001).

## Responsibility

- Route inbound messages from connectors (Zalo, internal, etc.) to the right destination.
- Fan-out messages across multiple channels.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, mongoose, platform-express, typeorm), `mongodb`, `redis`, `reflect-metadata`, `tslib`.
- **B-Platform SDKs**: `@b-platform-vn/sdk-mcm-common`, `@b-platform-vn/sdk-mcm-schemas`, `@b-platform-vn/sdk-mcm-streams`.
- **Datastores**: MongoDB, Redis.

## Public API surface

- HTTP service (health check at `/`, port 3001).

## Manifest contract

- `nest-cli.json`, `jest.config.js`, `app.yaml`, `Dockerfile`, `.npmrc`, `docs/`, `scripts/`, `tsconfig.*`.
- Local dev expects `docker-compose.local.yml` placed in the **parent** directory (clone all MCM services into one folder).

## Notes

- README is bilingual (Vietnamese + English), documents local ports.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
