# api-service-social

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-social` (planned) |
| **v3 target** | `api-service-social` (L2) — social domain |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Social domain service. Owns the **business logic** for content publishing and engagement — articles, posts, and comments. Distinct from [`api-service-content`](./api-service-content.md) (which is the B2C storefront content delivery via Strapi) — this service owns user-generated / editorial social content with engagement.

## Responsibility

- **Articles**: long-form editorial content, authoring, publishing lifecycle.
- **Posts**: short-form user-generated content.
- **Comments**: threaded comments on articles and posts, moderation.

## Dependencies

- **Consumers (L1)**: [`cfc-web-*`](./cfc-web-mdfoods.md) storefronts (article/post display), [`bof-web-bplatform`](./bof-web-bplatform.md) (editorial backoffice).
- **Cross-domain (via orchestrator)**: [`api-service-identity`](./api-service-identity.md) (author/commenter identity), [`api-service-content`](./api-service-content.md) (media attachments). Requests are routed through the Service Orchestrator, not called directly.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access. This service does **not** touch the datastore directly.

## Public API surface

- HTTP REST API for articles, posts, comments (CRUD + moderation).

## Notes

- ⚠️ This repo does **not exist** on the remote yet. New domain — no legacy implementation to fold.
- Boundary with [`api-service-content`](./api-service-content.md): content = storefront display content (Strapi-managed, read-only to storefronts); social = user/editorial-generated content with engagement (comments, posts). Confirm boundary before implementing.

## Owner

- Engineering (social team). Architecture: B-Platform architect owns this doc only.
