# api-mcm-connector-internal

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-mcm-connector-internal](https://github.com/b-platform-vn/api-mcm-connector-internal) |
| **v3 target** | `api-service-mcm` (L2) — fold with `api-mcm-omni-channel` into one MCM module service |
| **Layer** | API |
| **Status** | active |
| **Language** | TypeScript + JavaScript + Dockerfile |
| **Package** | `@b-platform-vn/api-mcm-connector-internal` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM **internal-channel** connector. NestJS service that handles internal message delivery & persistence — corresponds to the `internal-channel` package in the [`app-mcm`](./app-mcm.md) monorepo (`packages/internal-channel/`, port 3002).

## Responsibility

- Inbound/outbound internal-channel message routing.
- Persist messages to MongoDB + MSSQL.
- Publish/consume via RabbitMQ + Redis Streams.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, jwt, microservices, mongoose, passport, platform-express, typeorm), `amqplib`, `axios`, `mongodb`, `mongoose`, `mssql`, `passport-jwt`, `uuid`, `reflect-metadata`, `tslib`, `typeorm`, `upgrade`.
- **B-Platform SDKs**: `@b-platform-vn/sdk-mcm-common`, `@b-platform-vn/sdk-mcm-schemas`, `@b-platform-vn/sdk-mcm-streams`.
- **Datastores**: MongoDB, MSSQL, RabbitMQ, Redis.
- **Auth**: `passport-jwt`, consumes [`api-auth`](./api-auth.md).

## Public API surface

- HTTP + AMQP service; internal-only routes.

## Manifest contract

- `nest-cli.json`, `jest.config.js`, `app.yaml`, `Dockerfile`, `.npmrc`, `docs/`, `scripts/`, `tsconfig.*`.

## Notes

- README is empty on `main`. Doc gap.
- Shares the `@b-platform-vn/sdk-mcm-*` SDK trio with the other MCM connectors.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
