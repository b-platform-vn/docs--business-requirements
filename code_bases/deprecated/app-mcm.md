# app-mcm

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/app-mcm](https://github.com/b-platform-vn/app-mcm) |
| **v3 target** | *(retire after extraction)* — packages map to `cfc-web-mcm` + `api-service-mcm` + `api-service-zalo` + `sdk-mcm-schemas` |
| **Layer** | App (composite monorepo) |
| **Status** | active |
| **Language** | TypeScript (Nx monorepo) |
| **Package** | `@mcm/source` `0.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Multi-Channel Messaging (MCM) platform monorepo built with **Nx**. Brings together a Next.js web app, NestJS microservices, and third-party channel integrations under one workspace. This is the original / canonical MCM source; the standalone `api-mcm-*` and `web-mcm-messenger` repos appear to be extracted/parallel deployments of its packages.

## Responsibility

Bundle and run the full MCM stack:

- `packages/web-app/` — Next.js front-end (port 3000).
- `packages/omni-channel/` — NestJS message routing / fan-out (port 3001).
- `packages/internal-channel/` — NestJS internal delivery & persistence (port 3002).
- `packages/integrations/fb-integration/` — Facebook Messenger webhook (port 3011).
- `packages/integrations/zalo-integration/` — Zalo OA webhook (port 3012).
- `packages/integrations/wa-integration/` — WhatsApp Business webhook (port 3013).
- `libs/common/` — shared DTOs, interfaces, utilities.
- `libs/logger/` — shared logging module.
- `libs/message-broker/` — RabbitMQ abstractions.
- `libs/rxdb-schemas/` — RxDB schemas for offline-first local DB.
- `infra/docker-compose.yml` — local RabbitMQ, MongoDB, Redis.

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `@nestjs/*` (common, config, core, jwt, microservices, mongoose, passport, platform-express, typeorm), `rxdb`, `rxdb-server`, `amqplib`, `mongoose`, `mssql`, `redis`, `web-push`, `uuid`, `axios`, `rxjs`.
- **Infra**: RabbitMQ (`amqp://localhost:5672`, mgmt UI `:15672`), MongoDB (`:27017`), Redis (`:6379`).

## Public API surface

- Internal Nx workspace; produces deployable Next.js + NestJS services.

## Manifest contract

- Workspace root `package.json` + `tsconfig.base.json`; Nx-managed `packages/*` and `libs/*`. Includes `NPM_MONOREPO_GUIDE.md`.

## Notes

- Prereqs: Node 20 LTS, npm 10+, Docker Desktop.
- Local dev: `npm install` → `cp .env.example .env` → `docker compose -f infra/docker-compose.yml up -d` → run services.
- Authoritative README is detailed; this is the most documented repo in the org.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
