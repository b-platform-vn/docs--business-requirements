# dbo-queue

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-queue` |
| **v3 target** | `dbo-queue` (L3) — active |
| **Layer** | L3 — Database Operators |
| **Status** | active (repo created 2026-08-09) |
| **Role** | async ingress + cluster transfer job queue (no stack) |
| **Language** | NestJS 11 + TypeScript |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Optional ingress/async buffer for the DBO sub-layer. L2 services call [`dbo-head`](./dbo-head.md) directly for synchronous request/response. `dbo-queue` serves two async paths: (1) **bulk/background operations** from L2 (fire-and-forget writes, low-priority jobs), and (2) **cluster transfer jobs** produced by [`dbo-head`](./dbo-head.md)'s `ClusterTransferService` (R8) when a new clustered worker is registered and data must move from the default cluster → the new worker.

## Responsibility

- Accept async query/operation requests from L2 (bulk ops, background jobs only).
- Accept **cluster transfer jobs** from [`dbo-head`](./dbo-head.md) (R8) — read matching rows from the default-cluster worker, write them to the newly-registered clustered worker, verify row count, delete from source.
- Enqueue operations onto Redis Streams. The **synchronous path bypasses this queue** — L2 → [`dbo-head`](./dbo-head.md) directly.
- Full detail: [`technical-requirements/database-operation.md`](../technical-requirements/database-operation.md) §10.

## Dependencies

- **Runtime**: NestJS 11 + `@b-platform-vn/sdk-platform/dbo-streams`.
- **Downstream**: [`dbo-head`](./dbo-head.md).
- **For transfers** (R8): reads from the default-cluster worker + writes to the new clustered worker, both via [`dbo-head`](./dbo-head.md).
- **Reliability**: design TBD — the legacy `mcm-dlq-consumer` / `mcm-retry-scheduler` are retired; DBO reliability will be redesigned with the new MCM architecture.

## Public API surface

- `POST /enqueue` — L2 async path (bulk ops).
- Internal consumer for cluster transfer jobs (R8) — not directly callable by L2.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. ADR: `/memories/repo/dbo-architecture.md`. Technical requirements: [`../technical-requirements/database-operation.md`](../technical-requirements/database-operation.md).
- Transfer jobs are idempotent — a retried transfer re-checks which rows already exist in the target before copying, and re-checks which are gone from the source before deleting.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
