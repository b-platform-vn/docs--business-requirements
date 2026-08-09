# api-filestore

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/api-filestore](https://github.com/b-platform-vn/api-filestore) |
| **v3 target** | `api-service-filestore` (L2) — rename opportunistically |
| **Layer** | API |
| **Status** | active |
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
- **Auth**: `passport-jwt` (JWT), consumes [`api-auth`](./api-auth.md).

## Public API surface

- HTTP file upload/retrieve/delete endpoints (routes TBD).

## Manifest contract

- `nest-cli.json`, `Dockerfile`, `.npmrc` (GitHub Package Registry), `tsconfig.*`.

## Notes

- README is empty on `main`. Doc gap.

## Owner

- Engineering (platform team). Architecture: B-Platform architect owns this doc only.
