# dbo-worker-mongodb

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-worker-mongodb` |
| **v3 target** | `dbo-worker-mongodb` (L3) — active |
| **Layer** | L3 — Database Operators |
| **Status** | active (repo created 2026-08-09) |
| **Role** | dbo-worker (stack: `mongodb`) — declares mode + cluster condition per entity |
| **Language** | NestJS 11 + Mongoose 9 + TypeScript |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MongoDB adapter for the DBO sub-layer. Executes query plans against MongoDB databases. On boot, self-registers its supported entities + **mode** (`FullReadWrite` / `ClusteredReadWrite` / `FullWrite` / `FullRead` / `ClusteredWrite` / `ClusteredRead`) + optional **cluster condition** to [`dbo-metadata`](./dbo-metadata.md). Owns the MCM collections currently embedded across the `api-mcm-*` services.

## Responsibility

- **Self-register mode declarations** (R5), **Execute query plans**, **Migrations** (R4).
- Full detail: [`technical-requirements/database-operation.md`](../technical-requirements/database-operation.md) (§3, §4, §9).

## Dependencies

- **Runtime**: NestJS 11 + `mongoose` 9 + `@b-platform-vn/sdk-platform/dbo-schemas` + `…/dbo-streams`.
- **Upstream**: [`dbo-head`](./dbo-head.md) (only caller).
- **Catalog**: [`dbo-metadata`](./dbo-metadata.md) (registers declarations on boot).
- **Datastores**: MongoDB.

## Public API surface

- Internal L3-only interface: `execute(plan)`, `migrateUp/Down/Status` (R4), `getModeDeclarations()` (R5).
- Default-cluster instance (`isDefaultCluster: true`) deployed via [`platform-fluxcd`](./platform-fluxcd.md) (R7).

## Notes

- ⚠️ This repo does **not exist** on the remote yet. ADR: `/memories/repo/dbo-architecture.md`. Technical requirements: [`../technical-requirements/database-operation.md`](../technical-requirements/database-operation.md).
- Phase 3 of the DBO rollout (multi-engine).
- Shares schema ownership with the `mcm-schemas` subpath of [`sdk-platform`](./sdk-platform.md) (shared types) — this repo is the **operator** (runs migrations against real Mongo); the `sdk-platform/mcm-schemas` subpath is the **shared library** (types consumed in code). Don't merge them.
- A single worker instance declares **one mode per entity**; combine modes by running separate worker instances.

## Owner

- Engineering (MCM team / platform team). Architecture: B-Platform architect owns this doc only.
