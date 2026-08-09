# api-service-content

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-content` (planned) |
| **Current name** | `api-b2c-content` → [old doc](./deprecated/api-b2c-content.md) |
| **Folds** | File storage (`api-filestore` / `api-service-file`) folded in as media management |
| **v3 target** | `api-service-content` (L2) — rename opportunistically; absorbs file storage as part of content management |
| **Layer** | L2 — API Services |
| **Status** | planned (rename of `api-b2c-content`; absorbs `api-service-file`) |
| **Language** | TypeScript + Dockerfile |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Content management domain service. NestJS service backed by Strapi for storefront content delivery. Absorbs **file storage** (upload/retrieve/delete, metadata, blob storage) as part of content management — media files are content, not a separate cross-cutting domain.

## Responsibility

- **Content delivery** for public B2C storefronts (pages, banners, articles).
- **Media management** (f/k/a `api-service-file`) — upload/retrieve/delete files, file metadata in MSSQL, blob storage in MongoDB (GridFS-style), file events published to RabbitMQ.
- Likely the source behind the `API_BASE_URL` + `ID_KHOI` pattern used by AS Foods / LFarm.

## Dependencies

- **Runtime**: NestJS (`nest-cli.json`, `package.json`), `eslint.config.mjs`, `.prettierrc`.
- **Consumers (L1)**: [`cfc-web-asfoods`](./cfc-web-asfoods.md), [`cfc-web-lfarm`](./cfc-web-lfarm.md), [`cfc-web-di5`](./cfc-web-di5.md) (TBD confirm via `API_BASE_URL`), [`bof-web-bplatform`](./bof-web-bplatform.md).
- **Cross-domain (via orchestrator)**: consumed by all business domains for content/media attachments. Requests are routed through the Service Orchestrator, not called directly.
- **Message broker**: RabbitMQ (file events).
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (MongoDB for blobs, MSSQL for content metadata). This service does **not** touch the datastore directly.

## Public API surface

- HTTP content API (Strapi-backed). Specific routes TBD.
- HTTP file upload/retrieve/delete endpoints (JWT-protected).

## Notes

- README is the NestJS starter boilerplate. Doc gap: needs a product README.
- File storage is absorbed as a **media sub-domain** of content management, not a separate cross-cutting service. The legacy `api-filestore` repo becomes the file/media module within this service.

## Owner

- Engineering (content team). Architecture: B-Platform architect owns this doc only.
