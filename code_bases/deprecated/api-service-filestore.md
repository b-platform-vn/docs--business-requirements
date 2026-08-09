# api-service-filestore

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-filestore` (planned) |
| **Current name** | `api-filestore` → [old doc](./deprecated/api-filestore.md) |
| **v3 target** | `api-service-filestore` (L2) — rename opportunistically |
| **Layer** | L2 — API Services |
| **Status** | planned (rename of `api-filestore`) |
| **Language** | TypeScript + Dockerfile |
| **Package** | `@b-platform-vn/api-filestore` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

File storage service. NestJS app combining MongoDB (GridFS-style) + TypeORM (MSSQL metadata) + AMQP for async file events. JWT-protected.

## Responsibility

- Upload / retrieve / delete files.
- Publish file events to RabbitMQ.
- Store file metadata in MSSQL, blobs in MongoDB.

## Dependencies

- **Runtime**: `@nestjs/*` (common, config, core, jwt, microservices, mongoose, passport, platform-express, schedule, typeorm), `@types/amqplib`, `@types/mime-types`, `@types/passport-jwt`, `amqplib`, `axios`, `mime-types`, `mongodb`, `mongoose`, `mssql`, `passport-jwt`, `reflect-metadata`, `tslib`, `typeorm`, `uuid`.
- **Datastores**: MongoDB (blobs), MSSQL (metadata), RabbitMQ (events).
- **Database Operators (L3)**: [`dbo-worker-mongodb`](./dbo-worker-mongodb.md), [`dbo-worker-mssql`](./dbo-worker-mssql.md) (planned).
- **Auth**: `passport-jwt` (JWT), consumes [`api-service-auth`](./api-service-auth.md).

## Public API surface

- HTTP file upload/retrieve/delete endpoints (routes TBD).

## Notes

- README is empty on `main`. Doc gap.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
