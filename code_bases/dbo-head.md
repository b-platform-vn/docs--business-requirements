# dbo-head

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-head` |
| **v3 target** | `dbo-head` (L3) — active |
| **Layer** | L3 — Database Operators |
| **Status** | active (repo created 2026-08-09) |
| **Role** | planner + fan-out + consolidator + policy engine + cluster router + transfer orchestrator (no stack) |
| **Language** | NestJS 11 + TypeScript (Fastify adapter) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

The federated query planner + policy enforcement point + cluster router for the DBO sub-layer. Exposes an HTTP API (not L3-internal-only) so clients can reach it from local development without setting up datastores locally. Receives dbo operations from L2 (`api-service-*`) either **synchronously** (`POST /query`) or **asynchronously** via [`dbo-queue`](./dbo-queue.md). Resolves worker ownership and **cluster routing** via [`dbo-metadata`](./dbo-metadata.md), enforces **row-level / column-level / masking** policies before dispatching, fans out to [`dbo-worker-*`](./dbo-worker-mssql.md) in parallel, consolidates results (incl. cross-stack joins for relational queries), and applies post-processing masks. Also orchestrates **cluster transfers** (R8) when a new clustered worker is registered, and **data migrations** (R4) for CI/CD pipelines.

## Responsibility

- **Plan** (R2), **Cluster route** (R5–R7), **Enforce policy** (R3), **Dev route** (R1), **Fan-out + consolidate**, **Transfer** (R8), **Migrate** (R4).
- Full detail: [`technical-requirements/database-operation.md`](../technical-requirements/database-operation.md) (§2–§13).

## Dependencies

- **Runtime**: NestJS 11 + Fastify + `@b-platform-vn/sdk-platform/dbo-schemas` + `…/dbo-streams` + OpenTelemetry + Casbin (policy engine).
- **Auth**: JWT issued by [`api-service-identity`](./api-service-identity.md); claims = `{ tenant_id, role, dev_mode, target_worker? }`.
- **Cache**: Redis — hot entity metadata + policy cache.
- **Catalog**: [`dbo-metadata`](./dbo-metadata.md).
- **Workers**: [`dbo-worker-mssql`](./dbo-worker-mssql.md), [`dbo-worker-mongodb`](./dbo-worker-mongodb.md), [`dbo-worker-postgresql`](./dbo-worker-postgresql.md).
- **Queue**: [`dbo-queue`](./dbo-queue.md).

## Public API surface

- `POST /query`, `POST /enqueue`, `GET /requests/{id}`, `POST /migrations/{up|down|verify|dryrun}`, `GET /migrations/status`, `POST /transfers`, `GET /transfers/{id}`.
- Full spec: [`technical-requirements/database-operation.md`](../technical-requirements/database-operation.md) §12.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. ADR: `/memories/repo/dbo-architecture.md`. Technical requirements: [`../technical-requirements/database-operation.md`](../technical-requirements/database-operation.md).
- Reliability (retry/DLQ) — the legacy `mcm-dlq-consumer` / `mcm-retry-scheduler` are retired; DBO reliability will be redesigned with the new MCM architecture.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
