# cfc-web-mcm

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/cfc-web-mcm` (planned) |
| **Current name** | `web-mcm-messenger` → [old doc](./deprecated/web-mcm-messenger.md) |
| **v3 target** | `cfc-web-mcm` (L1 / cfc) — rename opportunistically |
| **Layer** | L1 — User Interaction (cfc) |
| **Status** | planned (rename of `web-mcm-messenger`) |
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
- **B-Platform SDKs (L0)**: [`sdk-platform`](./sdk-platform.md) subpaths — `@b-platform-vn/sdk-platform/mcm-common`, `…/mcm-schemas`.
- **Backend (L2)**: CRM communication APIs — [`api-service-crm`](./api-service-crm.md) (router) + connector services (`api-service-zalo`, `api-service-facebook`, `api-service-email`, `api-service-whatsapp`).
- **Build**: SWC (`.swcrc`); E2E tests in `e2e/`.

## Public API surface

- Web UI only.

## Notes

- Shares `@mcm/*` package scope with the legacy [`app-mcm`](./deprecated/app-mcm.md) monorepo — confirm whether this repo is the extracted/standalone version of `app-mcm`'s `packages/web-app/`.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
