# api-b2c-content

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-b2c-content](https://github.com/b-platform-vn/api-b2c-content) |
| **v3 target** | `api-service-content` (L2) — rename opportunistically |
| **Layer** | API |
| **Status** | active |
| **Language** | TypeScript + Dockerfile |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

B2C content API. NestJS service backed by Strapi (per `api-b2cstore` dep on `@strapi/client`). Serves storefront content (pages, banners, articles) to the public `web-b2c-*` storefronts (AS Foods, LFarm, Di5).

## Responsibility

- Content delivery for public B2C storefronts.
- Likely the source behind the `API_BASE_URL` + `ID_KHOI` pattern used by AS Foods / LFarm.

## Dependencies

- **Runtime**: NestJS (`nest-cli.json`, `package.json`), `eslint.config.mjs`, `.prettierrc`.
- **Consumers**: [`web-b2c-asfoods`](./web-b2c-asfoods.md), [`web-b2c-lfarm`](./web-b2c-lfarm.md), [`web-b2c-di5`](./web-b2c-di5.md) (TBD confirm via `API_BASE_URL`).

## Public API surface

- HTTP content API (Strapi-backed). Specific routes TBD.

## Notes

- README is the NestJS starter boilerplate. Doc gap: needs a product README.

## Owner

- Engineering (content team). Architecture: B-Platform architect owns this doc only.
