# mcm-dlq-consumer

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/mcm-dlq-consumer](https://github.com/b-platform-vn/mcm-dlq-consumer) |
| **v3 target** | retire — MCM solution to be reworked; DBO reliability will be designed as part of the new MCM architecture |
| **Layer** | MCM runtime service |
| **Status** | active |
| **Language** | TypeScript + JavaScript + Dockerfile |
| **Package** | `@b-platform-vn/mcm-dlq-consumer` `1.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM **dead-letter queue consumer**. Reads the `mcm:events:deadletter` Redis Stream and persists failed events to MongoDB `dead_letter_events`. Exposes Prometheus metrics (`prom-client`).

## Responsibility

- Drain the MCM DLQ.
- Persist dead-letter events for inspection/replay.
- Export Prometheus metrics.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, platform-express), `commander`, `mongodb`, `prom-client`, `reflect-metadata`, `tslib`.
- **B-Platform SDKs**: [`sdk-platform`](./sdk-platform.md) subpath — `@b-platform-vn/sdk-platform/mcm-streams`.
- **Datastores**: MongoDB, Redis (via `sdk-platform/mcm-streams`).

## Public API surface

- No HTTP routes (consumer process). Prometheus metrics endpoint only (TBD port).

## Manifest contract

- `nest-cli.json`, `Dockerfile`, `.npmrc`, `tsconfig.*`.
- Description on repo is authoritative: "MCM DLQ Consumer - reads mcm:events:deadletter, stores to MongoDB dead_letter_events".

## Notes

- README is empty on `main`; the GitHub repo description is the only documentation. Doc gap.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
