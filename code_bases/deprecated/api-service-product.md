# api-service-product

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-product` (planned) |
| **v3 target** | `api-service-product` (L2) — new repo |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Product module service. One of the installed apps in the B-Platform Super App (per `products/bplatform-general/architecture/super-app.md` — the Super App lists `B-Platform Product App` as an installed app). Owns product catalog management (PMS).

## Responsibility

- Product catalog management, product data, categories.

## Dependencies

- **Consumers (L1)**: [`bof-web-bplatform`](./bof-web-bplatform.md) (installed app in the Super App).
- **Datastores**: TBD.
- **Database Operator (L3)**: TBD.

## Public API surface

- HTTP REST API (routes TBD). Server actions invoked through the Super App BFF.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Listed in the Super App architecture as a planned installed app.

## Owner

- Engineering (product team). Architecture: B-Platform architect owns this doc only.
