# sdk-mcm-message-broker

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/sdk-mcm-message-broker](https://github.com/b-platform-vn/sdk-mcm-message-broker) |
| **v3 target** | → fold into `sdk-platform` (subpath: `/mcm-message-broker`) |
| **Layer** | SDK |
| **Status** | active |
| **Language** | TypeScript |
| **Package** | `@b-platform-vn/sdk-mcm-message-broker` `1.1.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

MCM RabbitMQ abstractions for NestJS. Wraps `amqplib` behind a shared NestJS module so MCM services share one broker contract.

## Responsibility

- RabbitMQ producer/consumer module for MCM services.

## Dependencies

- **Runtime**: `@nestjs/common`, `@nestjs/config`, `amqplib`, `@types/amqplib`, `tslib`.
- **Origin**: corresponds to `libs/message-broker/` in the [`app-mcm`](./app-mcm.md) monorepo.

## Public API surface

- NPM package `@b-platform-vn/sdk-mcm-message-broker` (GitHub Package Registry).
- CI badge: `Build and Publish SDK` GitHub Actions workflow.

## Manifest contract

- `src/`, `tsconfig.json`, `tsconfig.lib.json`, `.github/workflows/release.yml`.

## Owner

- Engineering (MCM team). Architecture: B-Platform architect owns this doc only.
