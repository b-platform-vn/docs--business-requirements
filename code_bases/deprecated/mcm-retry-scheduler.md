# mcm-retry-scheduler

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/mcm-retry-scheduler](https://github.com/b-platform-vn/mcm-retry-scheduler) |
| **v3 target** | retire — MCM solution to be reworked; DBO reliability will be designed as part of the new MCM architecture |
| **Layer** | MCM runtime service |
| **Status** | active |
| **Language** | TypeScript + Dockerfile |
| **Package** | `@b-platform-vn/mcm-retry-scheduler` `1.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM **retry scheduler**. A leader-elected singleton that polls the `mcm:retry:zset` Redis sorted set and replays events. Exposes Prometheus metrics.

## Responsibility

- Single-instance (leader-elected) scheduler.
- Poll retry queue and replay failed MCM events.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, platform-express), `prom-client`, `reflect-metadata`, `tslib`.
- **B-Platform SDKs**: [`sdk-platform`](./sdk-platform.md) subpath — `@b-platform-vn/sdk-platform/mcm-streams`.
- **Datastores**: Redis (via `sdk-platform/mcm-streams`).

## Public API surface

- No HTTP routes (scheduler process). Prometheus metrics endpoint only.

## Manifest contract

- `nest-cli.json`, `Dockerfile`, `.npmrc`, `tsconfig.*`.
- Description on repo is authoritative: "MCM Retry Scheduler - leader-elected singleton, polls mcm:retry:zset, replays events".

## Notes

- README is empty on `main`; GitHub repo description is the only documentation. Doc gap.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
