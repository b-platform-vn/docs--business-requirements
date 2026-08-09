# web-mcm-messenger

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/web-mcm-messenger](https://github.com/b-platform-vn/web-mcm-messenger) |
| **v3 target** | `cfc-web-mcm` (L1 / cfc) — rename opportunistically |
| **Layer** | Web frontend |
| **Status** | active |
| **Language** | TypeScript (Next.js + Tailwind) |
| **Package** | `@mcm/web-app` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Customer-messaging web app for the MCM (Multi-Channel Messaging) platform. Offline-first via RxDB + Dexie/PouchDB adapters.

## Responsibility

- Web UI for end-customers to read/send messages across MCM channels.
- Offline-first local DB sync.

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `rxdb`, `y-dexie`, `dexie`, `dexie-react-hooks`, `pouchdb-adapter-http`, `pouchdb-adapter-idb`, `@microsoft/fetch-event-source`, `react-hot-toast`, `react-infinite-scroll-component`.
- **B-Platform SDKs**: `@b-platform-vn/sdk-mcm-common`, `@b-platform-vn/sdk-mcm-schemas`.
- **Backend**: MCM APIs ([`api-mcm-omni-channel`](./api-mcm-omni-channel.md), [`api-mcm-connector-internal`](./api-mcm-connector-internal.md), [`api-mcm-connector-zalo`](./api-mcm-connector-zalo.md)).
- **Build**: SWC (`.swcrc`); E2E tests in `e2e/`.

## Public API surface

- Web UI only.

## Notes

- Shares `@mcm/*` package scope with [`app-mcm`](./app-mcm.md) monorepo — confirm whether this repo is the extracted/standalone version of `app-mcm`'s `packages/web-app/`.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
