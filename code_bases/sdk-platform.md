# sdk-platform

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/sdk-platform` (planned) |
| **Folds** | `sdk-design-system` + `sdk-mcm-common` + `sdk-mcm-message-broker` + `sdk-mcm-schemas` + `sdk-mcm-streams` + `sdk-offlinedb` + `sdk-system-design` → [old docs](./deprecated/sdk-design-system.md) |
| **v3 target** | `sdk-platform` (L0) — single shared package across all layers |
| **Layer** | L0 — Shared package across all layers |
| **Status** | planned (fold of all `sdk-*` repos into one shared package) |
| **Language** | TypeScript + CSS |
| **Package** | `@b-platform-vn/sdk-platform` (planned) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

The single cross-cutting shared package consumed by **all layers** (L1, L2, L3). Absorbs every former domain SDK (`sdk-design-system`, `sdk-mcm-common`, `sdk-mcm-message-broker`, `sdk-mcm-schemas`, `sdk-mcm-streams`, `sdk-offlinedb`, `sdk-system-design`) into one package with internal module boundaries.

## Responsibility

- **Design system** (`sdk-design-system`): React component library, Radix/MUI/shadcn stack, shared UI primitives.
- **MCM common** (`sdk-mcm-common`): MCM shared DTOs / utils.
- **MCM message broker** (`sdk-mcm-message-broker`): MCM RabbitMQ abstractions.
- **MCM schemas** (`sdk-mcm-schemas`): MCM typed schemas (incl. `libs/rxdb-schemas` from `app-mcm`).
- **MCM streams** (`sdk-mcm-streams`): Redis Streams wrapper for NestJS — owns `mcm:events:deadletter` stream + `mcm:retry:zset`.
- **Offline DB** (`sdk-offlinedb`): Offline-first DB client SDK (RxDB).
- **System design** (`sdk-system-design`): Base UI/layout for Zalo Mini Apps.

## Dependencies

- None upstream (L0 depends on nothing).

## Public API surface

- NPM package `@b-platform-vn/sdk-platform` (GitHub Package Registry).
- Internal subpath exports per former SDK (e.g. `@b-platform-vn/sdk-platform/design-system`, `…/mcm-streams`, `…/mcm-schemas`).

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Folds 7 existing `sdk-*` repos into one.
- **Internal module boundaries preserved** — the folded SDKs become subpath exports, not separate repos. Each former SDK's public API surface is preserved as a subpath within `sdk-platform`.
- Retire the `@bplatform/` and `@bplatform-store/` scopes — everything publishes under `@b-platform-vn/sdk-platform`.
- The DBO transport wrapper (forked from `sdk-mcm-streams`) will be a subpath of this package too — `@b-platform-vn/sdk-platform/dbo-streams` (planned).

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
