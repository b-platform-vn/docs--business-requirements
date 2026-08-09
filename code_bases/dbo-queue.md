# dbo-queue

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-queue` (planned) |
| **v3 target** | `dbo-queue` (L3) — new repo |
| **Layer** | L3 — Database Operators |
| **Status** | planned (new) |
| **Role** | ingress (no stack) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Optional ingress/async buffer for the DBO sub-layer. L2 services call [`dbo-head`](./dbo-head.md) directly for synchronous request/response. `dbo-queue` exists only for the async/fire-and-forget path (bulk operations, background jobs, low-priority writes) — it enqueues operations for `dbo-head` to consume later.

## Responsibility

- Accept async query/operation requests from L2 services (bulk ops, background jobs only).
- Enqueue operations onto the DBO transport (Redis Streams — `@b-platform-vn/sdk-platform/dbo-streams` subpath).
- Decouple L2 async callers from the planner.
- The **synchronous path bypasses this queue** — L2 → `dbo-head` directly.

## Dependencies

- **Runtime**: expected NestJS + `@b-platform-vn/sdk-platform/dbo-streams` (forked from the `mcm-streams` subpath of [`sdk-platform`](./sdk-platform.md)).
- **Downstream**: [`dbo-head`](./dbo-head.md).
- **Reliability**: design TBD — the legacy `mcm-dlq-consumer` / `mcm-retry-scheduler` are retired (MCM solution is being reworked); DBO reliability will be designed with the new MCM architecture.

## Public API surface

- HTTP enqueue endpoint(s) for L2 callers.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Phase 1 of the DBO rollout (see `/memories/repo/dbo-architecture.md` if recorded).
- Transport decision (deferred): Redis Streams initially; gRPC only if latency forces it.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
