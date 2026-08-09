# dbo-worker-postgresql

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-worker-postgresql` (planned) |
| **v3 target** | `dbo-worker-postgresql` (L3) — new repo |
| **Layer** | L3 — Database Operators |
| **Status** | planned (new) |
| **Role** | dbo-worker (stack: `postgresql`) |
| **Language** | TBD (expected NestJS + TypeORM + `pg` + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

PostgreSQL adapter for the DBO sub-layer. Executes query plans against PostgreSQL databases. Only created if the legacy `api-b2cstore` PG footprint stays on PostgreSQL after folding into [`api-service-ecom`](./api-service-ecom.md).

## Responsibility

- Translate logical query plans (from [`dbo-head`](./dbo-head.md)) into PostgreSQL-native queries (TypeORM + `pg`).
- Own PostgreSQL schema + migrations.

## Dependencies

- **Runtime**: expected NestJS + `typeorm` + `pg` + `@b-platform-vn/sdk-dbo-schemas` + `@b-platform-vn/sdk-dbo-streams`.
- **Upstream**: [`dbo-head`](./dbo-head.md) (only caller).
- **Datastores**: PostgreSQL.

## Public API surface

- Internal L3-only interface.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. **Conditional** — only create if the legacy `api-b2cstore` PG footprint stays. If the PG data is migrated to MSSQL (consolidating under `api-service-ecom`), this worker may never be needed.
- Verify the PG footprint before building.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
