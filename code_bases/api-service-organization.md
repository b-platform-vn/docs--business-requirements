# api-service-organization

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-organization` (planned) |
| **Folds** | `api-b2b-mdfoods` (B2B parts) + `api-backoffice-quotes` + `api-sale` → [old docs](./deprecated/api-service-mdfoods.md) |
| **v3 target** | `api-service-organization` (L2) — b2b domain |
| **Layer** | L2 — API Services |
| **Status** | planned (fold of `api-b2b-mdfoods` B2B parts + `api-backoffice-quotes` + `api-sale`) |
| **Language** | TBD (expected NestJS + TypeORM + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Organization (B2B) domain service. Owns the **business logic** for business-to-business operations — company management, employee/permission, sales pipeline, and quote management. Folds the B2B parts of `api-b2b-mdfoods`, plus `api-backoffice-quotes` and `api-sale`, into one domain-driven service.

## Responsibility

- **Employee**: employee accounts, roles, profiles (B2B side).
- **Company**: company info, company-member relationships.
- **Member/permission**: company member permissions, role assignment (from `api-b2b-mdfoods` FRDs 12–13).
- **Sales pipeline**: opportunities, stages, quote-to-order sales flow (from `api-sale`).
- **Quote management**: operator-side quote creation, approval, lifecycle (from `api-backoffice-quotes`).

## Dependencies

- **Consumers (L1)**: [`bof-web-bplatform`](./bof-web-bplatform.md) (backoffice portal), [`cfc-web-mdfoods`](./cfc-web-mdfoods.md) (MDFoods B2B storefront calls quote/sales flow).
- **Cross-domain (via orchestrator)**: [`api-service-identity`](./api-service-identity.md) (operator auth/SSO), [`api-service-ecom`](./api-service-ecom.md) (quote → order handoff). Requests are routed through the Service Orchestrator, not called directly.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (MSSQL). This service does **not** touch the datastore directly.

## Public API surface

- HTTP REST API for company, employee, member, quote, sales pipeline.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Reference implementations: `api-b2b-mdfoods` (B2B parts), `api-backoffice-quotes`, `api-sale` (planned — see Super App installed apps).
- B2B quote flow connects to [`api-service-ecom`](./api-service-ecom.md) for order conversion (quote-to-order).
- The Super App lists `B-Platform Sale App` as an installed app — that UI consumes this service.

## Owner

- Engineering (B2B team). Architecture: B-Platform architect owns this doc only.
