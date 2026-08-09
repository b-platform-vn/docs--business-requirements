# sdk-mcm-schemas

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/sdk-mcm-schemas](https://github.com/b-platform-vn/sdk-mcm-schemas) |
| **v3 target** | → fold into `sdk-platform` (subpath: `/mcm-schemas`) |
| **Layer** | SDK |
| **Status** | active |
| **Language** | TypeScript |
| **Package** | `@b-platform-vn/sdk-mcm-schemas` `2.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM typed schemas (runtime-validated message/event schemas) for the MCM platform. Largest of the MCM SDKs by code (~48k TS).

## Responsibility

- Authoritative message & event schemas shared by all MCM producers and consumers.

## Dependencies

- **Runtime**: `as-typed`, `tslib`.
- **Consumers**: every MCM connector + the omni-channel API + the DLQ/retry services + the messenger web app.

## Public API surface

- NPM package `@b-platform-vn/sdk-mcm-schemas` (GitHub Package Registry).
- CI badge: `Build and Publish SDK` GitHub Actions workflow.

## Manifest contract

- `src/`, `tsconfig.json`, `tsconfig.lib.json`, `.github/workflows/release.yml`.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
