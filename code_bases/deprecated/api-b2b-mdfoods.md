# api-b2b-mdfoods

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-b2b-mdfoods](https://github.com/b-platform-vn/api-b2b-mdfoods) |
| **v3 target** | `api-service-mdfoods` (L2) — rename opportunistically |
| **Layer** | API |
| **Status** | active |
| **Language** | TypeScript + Dockerfile |
| **Package** | `api-b2b-mdfoods` `0.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MDFoods B2B backend service. NestJS + TypeORM + MSSQL. Powers the [`web-b2b-mdfoods`](./web-b2b-mdfoods.md) storefront and implements the 20 MDFoods FRDs documented under `products/mdfoods/`.

## Responsibility

- Product catalog, search, cart, quote request, quote management, order/payment, order management.
- Registration, auth (via [`api-auth`](./api-auth.md) SSO), profile, addresses, company info, member/permission.
- In-app messages/notification, password recovery.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, platform-express, typeorm, cache-manager), `typeorm`, `mssql`, `@keyv/redis`, `redis`, `cache-manager`, `cacheable`, `class-transformer`, `class-validator`, `express-session`, `jsonwebtoken`, `@types/jsonwebtoken`, `reflect-metadata`, `rxjs`.
- **Frontend consumer**: [`web-b2b-mdfoods`](./web-b2b-mdfoods.md) via Next.js Server Actions.
- **Datastores**: MSSQL (primary), Redis (cache/session).

## Public API surface

- HTTP REST API consumed by the MDFoods B2B frontend.

## Manifest contract

- Standard NestJS app: `nest-cli.json`, `eslint.config.mjs`, `tsconfig.app.json`/`tsconfig.json`, `Dockerfile`, `.github/` CI.

## Notes

- README is empty on `main`. Doc gap.

## Owner

- Engineering (MDFoods team). Architecture: B-Platform architect owns this doc only.
