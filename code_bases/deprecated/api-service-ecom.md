# api-service-ecom

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-ecom` (planned) |
| **Current name** | `api-ecom-universal` → [old doc](./deprecated/api-ecom-universal.md) |
| **v3 target** | `api-service-ecom` (L2) — rename opportunistically |
| **Layer** | L2 — API Services |
| **Status** | planned (rename of `api-ecom-universal`) |
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
- **Database Operator (L3)**: [`dbo-worker-mssql`](./dbo-worker-mssql.md) (planned — migrates the ~46k TSQL).
- **External**: OpenAI, Resend.

## Public API surface

- HTTP REST API (routes TBD from `src/`).

## Notes

- The TSQL footprint suggests significant DB-level logic (stored procs / migrations) — migrates to [`dbo-worker-mssql`](./dbo-worker-mssql.md) under v3.
- README is empty on `main`. Doc gap given the size and centrality of this service.
- `.claude/` directory present (Claude Code agent config).

## Owner

- Engineering (e-commerce platform). Architecture: B-Platform architect owns this doc only.
