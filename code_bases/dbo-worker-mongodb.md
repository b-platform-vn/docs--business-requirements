# dbo-worker-mongodb

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-worker-mongodb` (planned) |
| **v3 target** | `dbo-worker-mongodb` (L3) — new repo |
| **Layer** | L3 — Database Operators |
| **Status** | planned (new) |
| **Role** | dbo-worker (stack: `mongodb`) |
| **Language** | TBD (expected NestJS + Mongoose + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MongoDB adapter for the DBO sub-layer. Executes query plans against MongoDB databases. Owns the MCM collections currently embedded across the `api-mcm-*` services.

## Responsibility

- Translate logical query plans (from [`dbo-head`](./dbo-head.md)) into MongoDB-native queries (Mongoose).
- Own MongoDB collection schemas + indexes.

## Dependencies

- **Runtime**: expected NestJS + `mongoose` + `@b-platform-vn/sdk-dbo-schemas` + `@b-platform-vn/sdk-dbo-streams`.
- **Upstream**: [`dbo-head`](./dbo-head.md) (only caller).
- **Datastores**: MongoDB.

## Public API surface

- Internal L3-only interface.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Phase 3 of the DBO rollout (multi-engine).
- Shares schema ownership with the `mcm-schemas` subpath of [`sdk-platform`](./sdk-platform.md) (shared types) — this repo is the **operator** (runs migrations against real Mongo); the `sdk-platform/mcm-schemas` subpath is the **shared library** (types consumed in code). Don't merge them.

## Owner

- Engineering (MCM team / platform team). Architecture: B-Platform architect owns this doc only.
