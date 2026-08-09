# dbo-head

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-head` (planned) |
| **v3 target** | `dbo-head` (L3) — new repo |
| **Layer** | L3 — Database Operators |
| **Status** | planned (new) |
| **Role** | planner + fan-out + result consolidator (no stack) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

The federated query planner. Receives dbo operations from L2 (`api-service-*`) — either **synchronously** (L2 calls `dbo-head` directly and waits for the response) or **asynchronously** via [`dbo-queue`](./dbo-queue.md) (for bulk/background ops). `dbo-head` queries [`dbo-metadata`](./dbo-metadata.md) for worker/entity ownership, delegates to one or more [`dbo-worker-*`](./dbo-worker-mssql.md) in parallel, and consolidates the final result back to the requester.

## Responsibility

- Plan: resolve which worker(s) own the entities in the operation.
- Fan-out: delegate sub-queries to the right workers (serial or parallel).
- Consolidate: merge partial results into the final response.
- Reliability (retry/DLQ) — design TBD as part of the new MCM architecture (the legacy `mcm-dlq-consumer` / `mcm-retry-scheduler` are retired, not carried forward).

## Dependencies

- **Runtime**: expected NestJS + `@b-platform-vn/sdk-dbo-streams` + `@b-platform-vn/sdk-dbo-schemas` (the query-plan DTO contract).
- **Catalog**: [`dbo-metadata`](./dbo-metadata.md).
- **Workers**: [`dbo-worker-mssql`](./dbo-worker-mssql.md), [`dbo-worker-mongodb`](./dbo-worker-mongodb.md), [`dbo-worker-postgresql`](./dbo-worker-postgresql.md).

## Public API surface

- Internal L3-only interface (consumes from `dbo-queue`, calls `dbo-worker-*`).

## Notes

- ⚠️ This repo does **not exist** on the remote yet. The MCM blueprint (`api-mcm-omni-channel`) is being reworked — DBO reliability design will follow the new MCM architecture, not the legacy `mcm-*` runtime.
- Query model decision (deferred): recommend logical Query Plan DTO (Option B), not raw SQL passthrough.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
