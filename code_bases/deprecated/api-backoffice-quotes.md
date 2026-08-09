# api-backoffice-quotes

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-backoffice-quotes](https://github.com/b-platform-vn/api-backoffice-quotes) |
| **v3 target** | `api-service-quote` (L2) — rename opportunistically; drop `backoffice` (audience forbidden on L2); singular form per convention |
| **Layer** | API |
| **Status** | active |
| **Language** | TypeScript + JavaScript |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Backoffice quotes service. NestJS application (per `nest-cli.json`, `src/`, `test/`). Likely the operator-side counterpart to the MDFoods B2B quote flow — manages quotes created via `api-b2b-mdfoods` from the backoffice portal.

## Responsibility

- Backoffice quote management (operator view).

## Dependencies

- **Runtime**: NestJS (`nest-cli.json`, `package.json`, `eslint.config.mjs`, `.prettierrc`).
- **Consumers**: likely [`web-admin-portal`](./web-admin-portal.md).

## Public API surface

- HTTP REST API (routes TBD).

## Notes

- README is the NestJS starter boilerplate. Doc gap.

## Owner

- Engineering (backoffice team). Architecture: B-Platform architect owns this doc only.
