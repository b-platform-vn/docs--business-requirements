# dbo-worker-mssql

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-worker-mssql` (planned) |
| **v3 target** | `dbo-worker-mssql` (L3) — new repo |
| **Layer** | L3 — Database Operators |
| **Status** | planned (new) |
| **Role** | dbo-worker (stack: `mssql`) |
| **Language** | TBD (expected NestJS + TypeORM + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MSSQL adapter for the DBO sub-layer. Executes query plans against MSSQL databases. Migrates the ~46k lines of TSQL currently embedded in [`api-service-ecom`](./api-service-ecom.md) (legacy `api-ecom-universal`).

## Responsibility

- Translate logical query plans (from [`dbo-head`](./dbo-head.md)) into MSSQL-native queries (TypeORM + raw TSQL).
- Own MSSQL schema + migrations + stored procedures.

## Dependencies

- **Runtime**: expected NestJS + `typeorm` + `mssql` + `@b-platform-vn/sdk-dbo-schemas` + `@b-platform-vn/sdk-dbo-streams`.
- **Upstream**: [`dbo-head`](./dbo-head.md) (only caller — L2 never calls workers directly).
- **Datastores**: MSSQL.

## Public API surface

- Internal L3-only interface (consumes query plans from `dbo-head`).

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Phase 1 MVP target — migrate one entity from `api-service-ecom` as the pilot.
- Phase 0 (contracts) must land first: `sdk-dbo-schemas` defines the query-plan DTO this worker translates.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
