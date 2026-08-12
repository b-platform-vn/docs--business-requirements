# api-service-ecom

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-ecom` |
| **Folds** | `api-ecom-universal` + `api-b2cstore` + `api-product` + `api-b2b-mdfoods` (B2C parts) → [old docs](./deprecated/api-service-ecom.md) |
| **v3 target** | `api-service-ecom` (L2) — b2c domain |
| **Layer** | L2 — API Services |
| **Status** | active (repo created 2026-08-12, NestJS scaffold — fold of `api-ecom-universal` + `api-b2cstore` + `api-product` + `api-b2b-mdfoods` B2C parts) |
| **Language** | NestJS 11 + TypeORM + TypeScript |
| **Default branch** | `main` |

Last synced: 2026-08-12

## Purpose

E-commerce (B2C) domain service. Owns the **business logic** for direct-to-consumer commerce across all B2C storefronts (MDFoods B2C, AS Foods, LFarm, Di5). Folds the legacy `api-ecom-universal`, `api-b2cstore`, `api-product`, and the B2C parts of `api-b2b-mdfoods` into one domain-driven service.

## Responsibility

- **Customer** (B2C): customer accounts, profile, addresses (customer-side).
- **Products**: product catalog, product data, search, recommendations.
- **Product categories**: category tree, classification.
- **Order**: cart, checkout, order lifecycle, payment.
- **Delivery**: fulfillment, shipping, delivery tracking.
- AI features (product search/recommendation/chat) via OpenAI — folded from `api-ecom-universal`.
- Transactional email via Resend — folded from `api-ecom-universal`.

## Dependencies

- **Consumers (L1)**: [`cfc-web-mdfoods`](./cfc-web-mdfoods.md), [`cfc-web-asfoods`](./cfc-web-asfoods.md), [`cfc-web-di5`](./cfc-web-di5.md), [`cfc-web-lfarm`](./cfc-web-lfarm.md).
- **Cross-domain (via orchestrator)**: [`api-service-identity`](./api-service-identity.md) (auth/SSO/customer identity), [`api-service-content`](./api-service-content.md) (storefront content + file storage). Requests are routed through the Service Orchestrator, not called directly.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (MSSQL for catalog/order, MongoDB for legacy store data). This service does **not** touch the datastore directly.

## Public API surface

- HTTP REST API for catalog, cart, checkout, order, payment, delivery.
- AI endpoints (search/recommendation/chat).

## Notes

- ✅ Repo exists on the remote (`github.com/b-platform-vn/api-service-ecom`, created 2026-08-12). NestJS 11 scaffold pushed. Package scope: `@b-platform-vn/api-service-ecom`. Consumes `@b-platform-vn/dbo-schemas@^0.1.4` from GitHub Packages. The reference implementation is `api-ecom-universal` (largest API: ~371k TS + 210k JS + 46k TSQL).
- **~46k TSQL migrates to [`dbo-worker-mssql`](./dbo-worker-mssql.md)** (via [`dbo-head`](./dbo-head.md)) — not to this service. This service owns the business logic that calls `dbo-head`; `dbo-head` in turn delegates to `dbo-worker-mssql`.
- B2B parts of `api-b2b-mdfoods` (company, employee, member/permission, quote management) move to [`api-service-organization`](./api-service-organization.md), **not** here.
- MDFoods B2C-specific features (registration, in-app messages, notification, password recovery) live here as customer-domain functions.

## Owner

- Engineering (e-commerce team). Architecture: B-Platform architect owns this doc only.
