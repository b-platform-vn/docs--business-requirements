# api-service-store

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-store` (planned) |
| **Current name** | `api-b2cstore` → [old doc](./deprecated/api-b2cstore.md) |
| **v3 target** | `api-service-store` (L2) — rename opportunistically; verify vs `api-service-ecom` before merging |
| **Layer** | L2 — API Services |
| **Status** | planned (rename of `api-b2cstore`) |
| **Language** | TypeScript + Dockerfile + Shell |
| **Package** | `@bplatform-store/api` `1.0.0` (scope to retire → `@b-platform-vn/*`) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

"B-Platform Store API" — Express + TypeORM service backed by MongoDB + PostgreSQL. README shows it serving `/odeli/products` and a `/store-api/*` route via `platform-endpoints`. May be the legacy store API; verify whether it is being superseded by [`api-service-ecom`](./api-service-ecom.md).

## Responsibility

- Product store API for Odeli and other B2C storefronts.
- Includes an MCP server (`@modelcontextprotocol/sdk`), Firebase Admin, QR code generation.

## Dependencies

- **Runtime**: `express`, `typeorm`, `mongodb`, `pg`, `@strapi/client`, `@modelcontextprotocol/sdk`, `firebase-admin`, `axios`, `cors`, `qrcode`, `lru-cache`, `dotenv`, `zod`, `reflect-metadata`.
- **Datastores**: MongoDB, PostgreSQL.
- **Database Operators (L3)**: [`dbo-worker-mongodb`](./dbo-worker-mongodb.md), [`dbo-worker-postgresql`](./dbo-worker-postgresql.md) (planned).
- **Routing**: [`platform-endpoints`](./platform-endpoints.md) proxies `/store-api/*` → `$STORE_API_ENDPOINT`.

## Public API surface

- HTTP REST: `/odeli/products`, `/store-api/*`.

## Notes

- `cfc-web-di5` README mentions `http://localhost:9000/api` — port 9000 matches `api-service-store` per its README. Likely the backend for Di5.

## Owner

- Engineering (store team). Architecture: B-Platform architect owns this doc only.
