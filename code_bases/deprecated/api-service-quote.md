# api-service-quote

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-quote` (planned) |
| **Current name** | `api-backoffice-quotes` → [old doc](./deprecated/api-backoffice-quotes.md) |
| **v3 target** | `api-service-quote` (L2) — rename opportunistically; drop `backoffice` (audience forbidden on L2); singular form per convention |
| **Layer** | L2 — API Services |
| **Status** | planned (rename of `api-backoffice-quotes`) |
| **Language** | TypeScript + JavaScript |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Quote service. NestJS application (per `nest-cli.json`, `src/`, `test/`). The operator-side counterpart to the MDFoods B2B quote flow — manages quotes created via [`api-service-mdfoods`](./api-service-mdfoods.md) from the backoffice portal.

## Responsibility

- Backoffice quote management (operator view).

## Dependencies

- **Runtime**: NestJS (`nest-cli.json`, `package.json`, `eslint.config.mjs`, `.prettierrc`).
- **Consumers (L1)**: likely [`bof-web-bplatform`](./bof-web-bplatform.md).

## Public API surface

- HTTP REST API (routes TBD).

## Notes

- README is the NestJS starter boilerplate. Doc gap.

## Owner

- Engineering (backoffice team). Architecture: B-Platform architect owns this doc only.
