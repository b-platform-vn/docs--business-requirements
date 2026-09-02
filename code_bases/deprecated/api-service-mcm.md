# api-service-mcm

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-mcm` (planned) |
| **Folded from** | `api-mcm-connector-internal` + `api-mcm-omni-channel` → [old docs](./deprecated/api-mcm-connector-internal.md), [old docs](./deprecated/api-mcm-omni-channel.md) |
| **v3 target** | → fold into `api-service-crm` as a communication sub-domain of CRM |
| **Layer** | L2 — API Services |
| **Status** | planned (fold of `api-mcm-connector-internal` + `api-mcm-omni-channel`) |
| **Language** | TypeScript + JavaScript + Dockerfile |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM (Multi-Channel Messaging) domain service — the **omni-channel router** (message routing / fan-out) and the **internal-channel connector** (internal delivery & persistence). Corresponds to `packages/omni-channel/` (port 3001) + `packages/internal-channel/` (port 3002) in the legacy [`app-mcm`](./deprecated/app-mcm.md) monorepo. Distinct from the channel connector services (`api-service-zalo`, `api-service-facebook`, `api-service-email`, `api-service-whatsapp`), which own 3rd-party transport — this service routes messages between internal destinations.

## Responsibility

- **Route inbound messages** from connectors (received via the dedicated connector services) to the right destination.
- **Fan-out messages** across multiple channels.
- **Internal-channel message delivery & persistence**.
- Reliability (DLQ/retry) — design TBD as part of the new MCM architecture (legacy `mcm-dlq-consumer` / `mcm-retry-scheduler` retired, not carried forward).

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, jwt, microservices, mongoose, passport, platform-express, typeorm), `amqplib`, `axios`, `mongodb`, `mongoose`, `mssql`, `passport-jwt`, `uuid`, `reflect-metadata`, `tslib`, `typeorm`, `redis`.
- **B-Platform SDKs (L0)**: [`sdk-platform`](./sdk-platform.md) subpaths — `@b-platform-vn/sdk-platform/mcm-common`, `…/mcm-schemas`, `…/mcm-streams`.
- **Upstream**: dedicated connector services (they push inbound messages here via Redis Streams).
- **Message broker**: RabbitMQ, Redis (streams).
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (MongoDB, MSSQL). This service does **not** touch the datastore directly.
- **Auth**: `passport-jwt`, consumes [`api-service-identity`](./api-service-identity.md).

## Public API surface

- HTTP + AMQP service; internal-only routes. Health check (port 3001).

## Notes

- ⚠️ Folding two repos into one is a non-trivial migration — confirm the two services can actually merge before renaming. If they must stay separate for operational reasons, keep `api-service-mcm-internal` + `api-service-mcm-omni` instead.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
