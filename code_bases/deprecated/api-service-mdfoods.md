# api-service-mdfoods

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-mdfoods` (planned) |
| **Current name** | `api-b2b-mdfoods` → [old doc](./deprecated/api-b2b-mdfoods.md) |
| **v3 target** | `api-service-mdfoods` (L2) — rename opportunistically; drop `b2b` |
| **Layer** | L2 — API Services |
| **Status** | planned (rename of `api-b2b-mdfoods`) |
| **Language** | TypeScript + Dockerfile |
| **Package** | `api-b2b-mdfoods` `0.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MDFoods B2B backend service. NestJS + TypeORM + MSSQL. Powers the [`cfc-web-mdfoods`](./cfc-web-mdfoods.md) storefront and implements the 20 MDFoods FRDs documented under `products/mdfoods/`.

## Responsibility

- Product catalog, search, cart, quote request, quote management, order/payment, order management.
- Registration, auth (via [`api-service-auth`](./api-service-auth.md) SSO), profile, addresses, company info, member/permission.
- In-app messages/notification, password recovery.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, platform-express, typeorm, cache-manager), `typeorm`, `mssql`, `@keyv/redis`, `redis`, `cache-manager`, `cacheable`, `class-transformer`, `class-validator`, `express-session`, `jsonwebtoken`, `@types/jsonwebtoken`, `reflect-metadata`, `rxjs`.
- **Frontend consumer (L1)**: [`cfc-web-mdfoods`](./cfc-web-mdfoods.md) via Next.js Server Actions.
- **Datastores**: MSSQL (primary), Redis (cache/session).
- **Database Operator (L3)**: [`dbo-worker-mssql`](./dbo-worker-mssql.md) (planned — migrates DB concerns out).

## Public API surface

- HTTP REST API consumed by the MDFoods B2B frontend.

## Notes

- README is empty on `main`. Doc gap.

## Owner

- Engineering (MDFoods team). Architecture: B-Platform architect owns this doc only.
