# sdk-mcm-streams

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/sdk-mcm-streams](https://github.com/b-platform-vn/sdk-mcm-streams) |
| **v3 target** | → fold into `sdk-platform` (subpath: `/mcm-streams`) |
| **Layer** | SDK |
| **Status** | active |
| **Language** | TypeScript |
| **Package** | `@b-platform-vn/sdk-mcm-streams` `1.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM **Redis Streams** wrapper for NestJS. Provides the stream primitives (`mcm:events:deadletter`, `mcm:retry:zset`) consumed by the DLQ consumer and retry scheduler.

## Responsibility

- Redis Streams producer/consumer groups for MCM eventing.
- Retry sorted-set (`mcm:retry:zset`) primitives.

## Dependencies

- **Runtime**: `ioredis`, `uuid`, `tslib`.
- **Peer**: `@nestjs/common`, `@nestjs/core`.
- **Dev**: `@b-platform-vn/sdk-mcm-schemas`, `ioredis-mock`, `jest`, `ts-jest`.
- **Consumers**: [`api-mcm-connector-internal`](./api-mcm-connector-internal.md), [`api-mcm-connector-zalo`](./api-mcm-connector-zalo.md), [`api-mcm-omni-channel`](./api-mcm-omni-channel.md), [`mcm-dlq-consumer`](./mcm-dlq-consumer.md), [`mcm-retry-scheduler`](./mcm-retry-scheduler.md).

## Public API surface

- NPM package `@b-platform-vn/sdk-mcm-streams` (GitHub Package Registry).
- Description on repo is authoritative: "B-Platform SDK - MCM Redis Streams wrapper (NestJS)".

## Manifest contract

- `src/`, `tsconfig.lib.json`, `.github/`, `package.json` (scripts: `build`, `test`).

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
