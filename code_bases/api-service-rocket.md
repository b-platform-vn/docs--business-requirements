# api-service-rocket

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-rocket` |
| **v3 target** | `api-service-rocket` (L2) — Rocket.Chat integration domain |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-09-02

## Purpose

Rocket.Chat integration service. Owns the platform's integration with `https://rocket.b-platform.vn`: outbound channel messaging, bot replies, webhook ingestion, channel lookup, and message delivery tracking.

## Responsibility

- **Outbound messaging** — send messages to Rocket.Chat rooms, threads, and direct messages.
- **Bot runtime** — handle bot commands, scripted replies, and agent-to-channel automation.
- **Inbound events** — ingest Rocket.Chat webhooks/events and normalize them for internal consumers.
- **Channel metadata** — resolve channels, users, and message targets needed for delivery.
- **Delivery lifecycle** — retry, dedupe, audit, and status tracking for sent messages.

## Dependencies

- **Consumers (L2)**: `api-service-agent` and any other internal service that needs Rocket.Chat delivery or bot interactions; requests should enter through the Service Orchestrator.
- **External system**: Rocket.Chat (`rocket.b-platform.vn`) REST APIs and webhooks.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (delivery logs, channel mapping, bot state). This service does **not** touch the datastore directly.

## Public API surface

- HTTP APIs for send-message, bot actions, channel lookup, and webhook/event ingestion.
- Internal delivery callbacks and status queries (shape TBD).

## Notes

- This service owns Rocket.Chat transport and bot plumbing, not AI reasoning.
- Rocket.Chat-facing actions from `api-service-agent` should be routed via `api-service-orchestrator`, not by direct service-to-service calls.

## Owner

- Engineering (platform integrations team). Architecture: B-Platform architect owns this doc only.
