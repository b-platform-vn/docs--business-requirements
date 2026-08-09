# dbo-metadata

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-metadata` (planned) |
| **v3 target** | `dbo-metadata` (L3) — new repo |
| **Layer** | L3 — Database Operators |
| **Status** | planned (new) |
| **Role** | worker/entity/mode catalog (no stack) |
| **Language** | TBD (config repo initially, may graduate to a service) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Keeps track of all available workers, which entities they manage, and in which mode (read / write / read-write, primary/replica). Consulted by [`dbo-head`](./dbo-head.md) to resolve entity ownership before delegating to workers.

## Responsibility

- Catalog worker → entity → mode mappings.
- Source of truth for which `dbo-worker-*` owns which entity.

## Dependencies

- **Consumers**: [`dbo-head`](./dbo-head.md).

## Public API surface

- Config manifest (static) initially; may graduate to a runtime service with worker self-registration + heartbeat.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Architect decision: start as a **static manifest repo** (deployed as a config map consumed by `dbo-head`); graduate to a runtime service only when ownership changes faster than deploy cadence.
- If `dbo-head` absorbs the registry role (worker self-registration), this repo folds into `dbo-head`. Confirm before building both.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
