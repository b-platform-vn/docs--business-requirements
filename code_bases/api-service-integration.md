# api-service-integration

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-integration` (planned) |
| **Folds** | `api-mcm-connector-zalo` + `fb-integration` + `wa-integration` → [old docs](./deprecated/api-service-zalo.md) |
| **v3 target** | `api-service-integration` (L2) — 3rd-party integrations domain |
| **Layer** | L2 — API Services |
| **Status** | planned (fold of `api-mcm-connector-zalo` + `fb-integration` + `wa-integration`) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Third-party integrations domain service. Owns all 3rd-party channel connectors — Zalo OA, Facebook Messenger, WhatsApp Business — as one integration layer. Each channel is a connector module within this service, not a separate repo. Folds `api-mcm-connector-zalo` and the FB/WA integration packages from the legacy [`app-mcm`](./deprecated/app-mcm.md) monorepo.

## Responsibility

- **Outbound messages (Zalo, Facebook, WhatsApp)** — transform the internal message structure into each 3rd-party message format and send them out. Keep track of sent-message status; ensure they can be sent successfully (retry on failure). **Audit every send**: who requested it, and what was sent.
- **Inbound messages (Zalo, Facebook, WhatsApp)** — receive webhooks, transform incoming messages into the internal message structure, and save them to the database via [`dbo-head`](./dbo-head.md).
- **Outbound/Inbound Email** — send outbound email as requested by other services (e.g. CRM broadcasts). Sync inboxes from an SMTP server and persist inbound email.
- **ZNS (Zalo Notification Service, Zalo only)** — send promotion messages to Zalo users via ZNS.

## Dependencies

- **Upstream**: external platform APIs (Zalo OA + ZNS, Facebook Graph, WhatsApp Business, SMTP/IMAP for Email).
- **Callers (L2, via orchestrator)**: [`api-service-crm`](./api-service-crm.md) and other services request outbound sends through the Service Orchestrator; inbound messages are persisted here, not routed onward.
- **Runtime**: expected NestJS + [`sdk-platform`](./sdk-platform.md) subpaths — `@b-platform-vn/sdk-platform/mcm-streams` + `…/mcm-schemas`.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (MongoDB for messages, audit logs, send status). This service does **not** touch the datastore directly.

## Public API surface

- HTTP webhook endpoints per channel (Zalo, Facebook, WhatsApp) for inbound.
- Internal send API for outbound messages (Zalo, Facebook, WhatsApp, ZNS) and outbound Email — called by [`api-service-crm`](./api-service-crm.md) and other services.
- SMTP/IMAP sync job for inbound Email.

## Notes

- ⚠️ This repo does **not exist** on the remote yet. Reference implementations: `api-mcm-connector-zalo` (standalone repo) + `app-mcm/packages/integrations/{fb,wa}-integration/`.
- Boundary with [`api-service-crm`](./api-service-crm.md): CRM owns the *intent* (who to message, what to say, templates, broadcasts, tickets); integration owns the *transport* (format transform, send, status tracking, audit, inbound persistence). CRM calls integration; integration never calls CRM.
- Adding a new channel = adding a new connector module inside this service, not a new repo.

## Owner

- Engineering (integration team). Architecture: B-Platform architect owns this doc only.
