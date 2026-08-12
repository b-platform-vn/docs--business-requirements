# api-service-identity

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-identity` |
| **Folds** | `api-auth` + `api-unigate` (planned) → [old docs](./deprecated/api-service-auth.md), [unigate](./deprecated/api-service-unigate.md) |
| **v3 target** | `api-service-identity` (L2) — identity domain |
| **Layer** | L2 — API Services |
| **Status** | active (repo created 2026-08-12, NestJS scaffold — fold of `api-auth` + `api-service-unigate`) |
| **Language** | NestJS 11 + TypeScript |
| **Default branch** | `main` |

Last synced: 2026-08-12

## Purpose

Identity domain service. Owns the **business logic** for authentication, SSO, and customer identity across all B-Platform products — both the customer-facing SSO surface (UniGate) and the operator auth surface. Folds `api-auth` and the planned `api-service-unigate` into one cross-cutting identity domain service.

## Responsibility

- **Authentication**: sign-in, sign-up, token issuance, token validation.
- **SSO**: cross-product single sign-on (UniGate domain SSO cookie + per-product first-party sessions).
- **Customer identity**: customer account administration (customer-facing side).
- **Operator auth**: token validation for backoffice services.

## Dependencies

- **Consumers (L1)**: every `cfc-web-*` storefront (customer SSO), [`bof-web-bplatform`](./bof-web-bplatform.md) (operator auth).
- **Cross-domain (via orchestrator)**: consumed by all business domains ([`api-service-ecom`](./api-service-ecom.md), [`api-service-organization`](./api-service-organization.md), [`api-service-crm`](./api-service-crm.md)) for auth/identity. Requests are routed through the Service Orchestrator, not called directly.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (MSSQL for identity). This service does **not** touch the datastore directly.

## Public API surface

- HTTP auth/SSO endpoints (sign-in, sign-up, token validation, customer account admin).
- HttpOnly UniGate-domain SSO cookie + per-product first-party sessions.

## Notes

- ✅ Repo exists on the remote (`github.com/b-platform-vn/api-service-identity`, created 2026-08-12). NestJS 11 scaffold pushed. Package scope: `@b-platform-vn/api-service-identity`. Consumes `@b-platform-vn/dbo-schemas@^0.1.4` from GitHub Packages. Reference implementations: `api-auth` (operator auth) + `api-service-unigate` (planned — customer SSO).
- Per repo memory (`/memories/repo/unigate.md`): customer auth redirects to UniGate and returns through allowlisted callbacks; prefer an HttpOnly UniGate-domain SSO cookie plus per-product first-party sessions over a broadly shared parent-domain cookie.
- Cross-cutting by design — auth/SSO is not e-commerce business logic, so it doesn't fold into `api-service-ecom`.

## Owner

- Engineering (platform/auth team). Architecture: B-Platform architect owns this doc only.
