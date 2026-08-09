# sdk-mcm-common

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/sdk-mcm-common](https://github.com/b-platform-vn/sdk-mcm-common) |
| **v3 target** | → fold into `sdk-platform` (subpath: `/mcm-common`) |
| **Layer** | SDK |
| **Status** | active |
| **Language** | TypeScript |
| **Package** | `@b-platform-vn/sdk-mcm-common` `1.0.3` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM shared DTOs, interfaces, and utilities. The base shared library consumed by every MCM API and the MCM web app.

## Responsibility

- Shared types/utilities for the MCM platform.

## Dependencies

- **Runtime**: `tslib`.
- **Consumers**: [`api-mcm-connector-internal`](./api-mcm-connector-internal.md), [`api-mcm-connector-zalo`](./api-mcm-connector-zalo.md), [`api-mcm-omni-channel`](./api-mcm-omni-channel.md), [`web-mcm-messenger`](./web-mcm-messenger.md), [`app-mcm`](./app-mcm.md).

## Public API surface

- NPM package `@b-platform-vn/sdk-mcm-common` (GitHub Package Registry).
- CI badge: `Build and Publish SDK` GitHub Actions workflow.

## Manifest contract

- `src/`, `tsconfig.json`, `tsconfig.lib.json`, `.github/workflows/release.yml`.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
