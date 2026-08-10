# dbo-worker-mssql

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-worker-mssql` |
| **v3 target** | `dbo-worker-mssql` (L3) — active |
| **Layer** | L3 — Database Operators |
| **Status** | active (repo created 2026-08-09) |
| **Role** | dbo-worker (stack: `mssql`) — declares mode + cluster condition per entity |
| **Language** | NestJS 11 + TypeORM + TypeScript |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MSSQL adapter for the DBO sub-layer. Executes query plans against MSSQL databases. On boot, self-registers its supported entities + **mode** (`FullReadWrite` / `ClusteredReadWrite` / `FullWrite` / `FullRead` / `ClusteredWrite` / `ClusteredRead`) + optional **cluster condition** to [`dbo-metadata`](./dbo-metadata.md). Migrates the ~46k lines of TSQL currently embedded in [`api-service-ecom`](./api-service-ecom.md) (legacy `api-ecom-universal`).

## Responsibility

- **Self-register mode declarations** (R5), **Execute query plans**, **Migrations** (R4).
- Full detail: [`technical-requirements/database-operation.md`](../technical-requirements/database-operation.md) (§3, §4, §9).

## Dependencies

- **Runtime**: NestJS 11 + `typeorm` + `mssql` driver + `@b-platform-vn/sdk-platform/dbo-schemas` + `…/dbo-streams`.
- **Upstream**: [`dbo-head`](./dbo-head.md) (only caller — L2 never calls workers directly).
- **Catalog**: [`dbo-metadata`](./dbo-metadata.md) (registers declarations on boot).
- **Datastores**: MSSQL.

## Public API surface

- Internal L3-only interface: `execute(plan)`, `migrateUp/Down/Status` (R4), `getModeDeclarations()` (R5).
- Default-cluster instance (`isDefaultCluster: true`) deployed via [`platform-fluxcd`](./platform-fluxcd.md) (R7).

## Notes

- ⚠️ This repo does **not exist** on the remote yet. ADR: `/memories/repo/dbo-architecture.md`. Technical requirements: [`../technical-requirements/database-operation.md`](../technical-requirements/database-operation.md).
- Phase 1 MVP target — migrate one entity from `api-service-ecom` as the pilot.
- Phase 0 (contracts) must land first: `sdk-platform/dbo-schemas` defines the Query Plan DTO + `EntityModeDeclaration` this worker registers.
- A single worker instance declares **one mode per entity**; combine modes by running separate worker instances.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
