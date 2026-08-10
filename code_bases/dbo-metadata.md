# dbo-metadata

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/dbo-metadata` |
| **v3 target** | `dbo-metadata` (L3) — active |
| **Layer** | L3 — Database Operators |
| **Status** | active (repo created 2026-08-09) |
| **Role** | worker/entity/mode/policy/cluster catalog (no stack) |
| **Language** | TBD (config repo initially → runtime service with self-registration) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Source of truth for worker → entity → **mode** → **cluster condition** → **policy** mappings. Consulted by [`dbo-head`](./dbo-head.md) to resolve entity ownership, cluster routing, and policy enforcement before delegating to workers. Workers self-register their `EntityModeDeclaration[]` on boot; [`dbo-head`](./dbo-head.md) reads (cached) on every query.

## Responsibility

- **Catalog worker → entity → mode mappings** (R5), **Validate cluster conditions** (R6), **`DefaultClusterGuard`** (R7), **Policy slice** (R3), **Detect new clusters** (R8).
- Full detail: [`technical-requirements/database-operation.md`](../technical-requirements/database-operation.md) (§3–§5, §7).

## Dependencies

- **Consumers**: [`dbo-head`](./dbo-head.md) (cached reads), [`platform-fluxcd`](./platform-fluxcd.md) (deploys the default-cluster worker per stack).
- **Registrants**: every [`dbo-worker-*`](./dbo-worker-mssql.md) self-registers on boot.
- **Runtime**: NestJS when it graduates to a service; static YAML + config map for MVP.

## Public API surface

- `GET /ownership`, `GET /policies`, `GET /mode-declarations`, `POST /register`, `GET /cluster-conditions`, emits new-cluster-detected event.
- Full spec: [`technical-requirements/database-operation.md`](../technical-requirements/database-operation.md) §4.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. ADR: `/memories/repo/dbo-architecture.md`. Technical requirements: [`../technical-requirements/database-operation.md`](../technical-requirements/database-operation.md).
- MVP: static manifest deployed as a config map. Graduate to a runtime service with self-registration + heartbeat when ownership/cluster changes faster than deploy cadence.
- If [`dbo-head`](./dbo-head.md) absorbs the registry role, this repo folds into `dbo-head`. Policy + cluster slices stay here regardless.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
