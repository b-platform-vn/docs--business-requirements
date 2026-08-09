# api-service-unigate

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-unigate` (planned) |
| **v3 target** | `api-service-unigate` (L2) — new repo |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

UniGate customer identity, sign-up/sign-in, cross-product SSO, and customer-facing product access. The customer-side counterpart to the back-office [`bof-web-bplatform`](./bof-web-bplatform.md) management surface. See `products/unigate/` for the FRDs.

## Responsibility

- Customer authentication / SSO / product-access authorization across connected customer-facing products.
- Customer account administration (customer-facing side).

## Dependencies

- **Consumers (L1)**: all `cfc-*` storefronts (MDFoods, AS Foods, Di5, LFarm) redirect to UniGate for sign-in.
- **Datastores**: TBD — likely MSSQL for customer identity.
- **Database Operator (L3)**: [`dbo-worker-mssql`](./dbo-worker-mssql.md) (planned).

## Public API surface

- HTTP auth/SSO endpoints. HttpOnly UniGate-domain SSO cookie + per-product first-party sessions.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Per repo memory (`/memories/repo/unigate.md`): customer auth redirects to UniGate and returns through allowlisted callbacks; prefer an HttpOnly UniGate-domain SSO cookie plus per-product first-party sessions over a broadly shared parent-domain cookie.
- `B-Platform / UniGate` (management side) is a separate concern, exposed via the Super App — not a separate L2 repo.

## Owner

- Engineering (platform/auth). Architecture: B-Platform architect owns this doc only.
