# api-service-crm

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-crm` (planned) |
| **Folds** | MCM communication features (omni-channel router, internal-channel, DLQ/retry) folded in as a CRM communication sub-domain |
| **v3 target** | `api-service-crm` (L2) — new repo; absorbs MCM as a communication feature |
| **Layer** | L2 — API Services |
| **Status** | planned (new; absorbs `api-service-mcm`) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

CRM domain service. Implements the B-Platform CRM domain documented under `products/crm/`. The CRM portal is served at `crm.b-platform.vn.internal`. Absorbs the **MCM (Multi-Channel Messaging)** platform as a communication feature of CRM — customer interactions, conversations, and omnichannel messaging are part of customer relationship management.

## Responsibility

- **Customer information** — manage customer profiles, contact data, and relationships.
- **Support tickets** — receive and manage support ticket requests from customers (intake, tracking, resolution).
- **Customer communication** — communicate with customers via **Email** and **Message**. Outbound sends are dispatched through [`api-service-integration`](./api-service-integration.md): Internal → Zalo, Internal → Facebook, Internal → Notification center apps. Inbound replies are persisted by integration and read back via [`dbo-head`](./dbo-head.md).
- **Communication templates/scripts** — manage reusable message templates and agent scripts for consistent customer outreach.
- **Broadcast** — send promotions and announcements to customer segments via Email and Message channels.

## Dependencies

- **Consumers (L1)**: [`bof-web-bplatform`](./bof-web-bplatform.md) (operator CRM portal), [`cfc-web-mcm`](./cfc-web-mcm.md) (customer messaging web).
- **Channel dispatch (via orchestrator)**: [`api-service-integration`](./api-service-integration.md) — CRM requests outbound sends (Email, Zalo, Facebook, WhatsApp, ZNS) through the Service Orchestrator; integration persists inbound messages to the DB and CRM reads them back via [`dbo-head`](./dbo-head.md).
- **Cross-domain (via orchestrator)**: [`api-service-identity`](./api-service-identity.md) (auth), [`api-service-content`](./api-service-content.md) (media attachments for messages).
- **B-Platform SDKs (L0)**: [`sdk-platform`](./sdk-platform.md) subpaths — `@b-platform-vn/sdk-platform/mcm-common`, `…/mcm-schemas`, `…/mcm-streams`.
- **Message broker**: RabbitMQ, Redis (streams).
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (MongoDB for messages, MSSQL for CRM data). This service does **not** touch the datastore directly.

## Public API surface

- HTTP REST API for CRM data, interactions, pipeline (routes TBD).
- Internal message-routing endpoints (consumes from Redis Streams, calls `dbo-head`).

## Notes

- ⚠️ This repo does **not exist** on the remote yet. The `crm.b-platform.vn.internal` domain is reserved, indicating the CRM portal is planned.
- MCM is absorbed as a **communication sub-domain** of CRM, not a separate service. The legacy MCM runtime (`mcm-dlq-consumer`, `mcm-retry-scheduler`) is retired — reliability will be redesigned within the CRM service.

## Owner

- Engineering (CRM team). Architecture: B-Platform architect owns this doc only.
