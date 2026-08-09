# api-ecom-universal

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-ecom-universal](https://github.com/b-platform-vn/api-ecom-universal) |
| **v3 target** | `api-service-ecom` (L2) — rename opportunistically |
| **Layer** | API |
| **Status** | active |
| **Language** | TypeScript + JavaScript + TSQL + Shell + Dockerfile |
| **Package** | `api-bplatform-ecom-universal` `0.2.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Universal e-commerce API for the B-Platform ecosystem. NestJS + TypeORM + MSSQL + Redis, with AI (OpenAI via `@ai-sdk/openai` / `ai`) and Resend email integration. The largest API repo (~371k TS + ~210k JS + ~46k TSQL). Most recently updated API (pushed 2026-08-09).

## Responsibility

- Unified e-commerce backend (catalog, cart, order, payment) across B2C divisions.
- AI features (likely product search/recommendation/chat) via OpenAI.
- Transactional email via Resend.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, platform-express, typeorm), `typeorm`, `mssql`, `ioredis`, `redis`, `@ai-sdk/openai`, `ai`, `jsonwebtoken`, `resend`, `class-transformer`, `class-validator`, `zod`, `reflect-metadata`, `rxjs`.
- **Datastores**: MSSQL (with `TSQL` migrations under `tests/`), Redis.
- **External**: OpenAI, Resend.

## Public API surface

- HTTP REST API (routes TBD from `src/`).

## Manifest contract

- `nest-cli.json`, `app.yaml`, `docker-compose.yml` + `docker-compose.test.yml`, `.env.example` + `.env.test`, `docs/`, `scripts/`, `tests/`, `tsconfig.*`.
- `.npmrc` present (consumes `@b-platform-vn/*` packages from GitHub Package Registry).
- `.claude/` directory present (Claude Code agent config).

## Notes

- The TSQL footprint suggests significant DB-level logic (stored procs / migrations) — flag for review.
- README is empty on `main`. Doc gap given the size and centrality of this service.

## Owner

- Engineering (e-commerce platform). Architecture: B-Platform architect owns this doc only.
