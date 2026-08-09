# api-b2cstore

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-b2cstore](https://github.com/b-platform-vn/api-b2cstore) |
| **v3 target** | `api-service-store` (L2) — rename opportunistically; verify vs `api-service-ecom` before merging |
| **Layer** | API |
| **Status** | active (legacy?) |
| **Language** | TypeScript + Dockerfile + Shell |
| **Package** | `@bplatform-store/api` `1.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

"B-Platform Store API" — Express + TypeORM service backed by MongoDB + PostgreSQL. README shows it serving `/odeli/products` and a `/store-api/*` route via `platform-endpoints`. May be the legacy store API; verify whether it is being superseded by `api-ecom-universal`.

## Responsibility

- Product store API for Odeli and other B2C storefronts.
- Includes an MCP server (`@modelcontextprotocol/sdk`), Firebase Admin, QR code generation.

## Dependencies

- **Runtime**: `express`, `typeorm`, `mongodb`, `pg`, `@strapi/client`, `@modelcontextprotocol/sdk`, `firebase-admin`, `axios`, `cors`, `qrcode`, `lru-cache`, `dotenv`, `zod`, `reflect-metadata`.
- **Datastores**: MongoDB, PostgreSQL.
- **Routing**: `platform-endpoints` proxies `/store-api/*` → `$STORE_API_ENDPOINT`.

## Public API surface

- HTTP REST: `/odeli/products`, `/store-api/*`.

## Manifest contract

- `app/`, `docker-compose.yml`, `Local.Dockerfile`, `Dockerfile`, `scripts/`, `docs/`, `.env.template`.
- Scripts use `yarn.lock` (yarn-based).

## Notes

- `api-b2c-di5` README mentions `http://localhost:9000/api` — port 9000 matches `api-b2cstore` per its README ("Server is running on http://localhost:9000"). Likely the backend for Di5.

## Owner

- Engineering (store team). Architecture: B-Platform architect owns this doc only.
