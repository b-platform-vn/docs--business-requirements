# sdk-offlinedb

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/sdk-offlinedb](https://github.com/b-platform-vn/sdk-offlinedb) |
| **v3 target** | → fold into `sdk-platform` (subpath: `/offlinedb`) |
| **Layer** | SDK |
| **Status** | active |
| **Language** | TypeScript + JavaScript |
| **Package** | `@b-platform-vn/sdk-offlinedb` `1.0.2` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Offline DB client SDK. A TypeScript SDK for managing offline database operations (CRUD against a remote `apiUrl` with API-key auth). Has the most complete README of any B-Platform SDK.

## Responsibility

- `OfflineDBClient` with `getRecord`, `createRecord`, `updateRecord`, `deleteRecord`, `listRecords`.

## Dependencies

- **Runtime**: none (zero prod deps).
- **Dev**: `jest`, `ts-jest`, `typescript`, `eslint` + `prettier`, `@typescript-eslint/*`, `eslint-config-prettier`, `eslint-plugin-prettier`.

## Public API surface

- NPM package `@b-platform-vn/sdk-offlinedb` (public, not scoped to GitHub Package Registry — `private: null`).
- Config: `OfflineDBConfig { apiUrl, apiKey, timeout, debug }`.

## Manifest contract

- Dual build: `build:cjs` + `build:esm` + `build:types`. `CHANGELOG.md`, `CONTRIBUTING.md`, `jest.config.js`, `.eslintrc.js`, `.prettierrc.js`, `.editorconfig`, `.npmignore`.

## Notes

- Standalone SDK — no B-Platform internal package consumers identified yet (verify with a Pylance-style import scan if needed).

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
