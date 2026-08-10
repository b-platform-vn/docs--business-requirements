# dbo-worker-postgresql

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-worker-postgresql` |
| **v3 target** | `dbo-worker-postgresql` (L3) — active (conditional) |
| **Layer** | L3 — Database Operators |
| **Status** | active (repo created 2026-08-09, **conditional**) |
| **Role** | dbo-worker (stack: `postgresql`) — declares mode + cluster condition per entity |
| **Language** | NestJS 11 + TypeORM + `pg` + TypeScript |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

PostgreSQL adapter for the DBO sub-layer. Executes query plans against PostgreSQL databases. On boot, self-registers its supported entities + **mode** (`FullReadWrite` / `ClusteredReadWrite` / `FullWrite` / `FullRead` / `ClusteredWrite` / `ClusteredRead`) + optional **cluster condition** to [`dbo-metadata`](./dbo-metadata.md). Only created if the legacy `api-b2cstore` PG footprint stays on PostgreSQL after folding into [`api-service-ecom`](./api-service-ecom.md).

## Responsibility

- **Self-register mode declarations** (R5), **Execute query plans**, **Migrations** (R4).
- Full detail: [`technical-requirements/database-operation.md`](../technical-requirements/database-operation.md) (§3, §4, §9).

## Dependencies

- **Runtime**: NestJS 11 + `typeorm` + `pg` + `@b-platform-vn/sdk-platform/dbo-schemas` + `…/dbo-streams`.
- **Upstream**: [`dbo-head`](./dbo-head.md) (only caller).
- **Catalog**: [`dbo-metadata`](./dbo-metadata.md) (registers declarations on boot).
- **Datastores**: PostgreSQL.

## Public API surface

- Internal L3-only interface: `execute(plan)`, `migrateUp/Down/Status` (R4), `getModeDeclarations()` (R5).
- Default-cluster instance (`isDefaultCluster: true`) deployed via [`platform-fluxcd`](./platform-fluxcd.md) (R7).

## Notes

- ⚠️ This repo does **not exist** on the remote yet. ADR: `/memories/repo/dbo-architecture.md`. Technical requirements: [`../technical-requirements/database-operation.md`](../technical-requirements/database-operation.md).
- **Conditional** — only create if the legacy `api-b2cstore` PG footprint stays. If the PG data is migrated to MSSQL (consolidating under `api-service-ecom`), this worker may never be needed. Verify the PG footprint before building.
- A single worker instance declares **one mode per entity**; combine modes by running separate worker instances.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
