# api-service-file

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-file` (planned) |
| **Current name** | `api-filestore` → [old doc](./deprecated/api-service-filestore.md) |
| **v3 target** | → fold into `api-service-content` as media/file management sub-domain |
| **Layer** | L2 — API Services |
| **Status** | planned (rename + singular form; was `api-service-filestore`, originally `api-filestore`) |
| **Language** | TypeScript + JavaScript |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

File storage domain service. Cross-cutting — every business domain (e-commerce, organization, social, CRM) needs file storage. Combines MongoDB (GridFS-style blobs) + TypeORM (MSSQL metadata) + AMQP for async file events. JWT-protected.

## Responsibility

- **Upload / retrieve / delete files**.
- **File metadata** in MSSQL (business metadata: owner, domain, type).
- **Blob storage** in MongoDB (GridFS-style).
- **File events** published to RabbitMQ (async processing).

## Dependencies

- **Consumers (L1)**: [`cfc-web-*`](./cfc-web-mdfoods.md) storefronts, [`bof-web-bplatform`](./bof-web-bplatform.md).
- **Cross-domain**: consumed by all business domains for file attachments.
- **Message broker**: RabbitMQ (file events).
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (MongoDB for blobs, MSSQL for metadata). This service does **not** touch the datastore directly.

## Public API surface

- HTTP REST API for file upload/retrieve/delete (JWT-protected).

## Notes

- ⚠️ Current remote repo name is `api-filestore` — v3 target `api-service-file` follows the singular form convention (was `api-service-filestore`, corrected to `file`).
- Cross-cutting by design — file storage has business metadata but no single owning business domain, so it stays as its own cross-cutting L2 service rather than folding into e-commerce or organization.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
