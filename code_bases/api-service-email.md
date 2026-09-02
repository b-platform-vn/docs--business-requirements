# api-service-email

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-email` |
| **v3 target** | `api-service-email` (L2) — Email integration connector |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-09-02

## Purpose

Email integration connector service. Owns outbound and inbound email communication for the B-Platform ecosystem.

## Responsibility

- **Outbound email** — send messages for CRM, notifications, and other approved workflows.
- **Inbound email** — receive mailbox/webhook/IMAP events and normalize them into internal message structures.
- **Delivery audit** — track who requested the send, what was sent, and the delivery result.
- **Message state** — maintain delivery, retry, and deduplication state.

## Dependencies

- **Callers (L2, via orchestrator)**: services that need email communication, primarily `api-service-crm`.
- **Upstream system**: SMTP/IMAP and email provider APIs.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (delivery logs, inbox state, message state). This service does **not** touch the datastore directly.

## Public API surface

- HTTP webhook endpoints for inbound email events.
- Internal send APIs for outbound email delivery.
- Delivery-status query endpoints (shape TBD).

## Notes

- This service is the Email connector boundary, not the CRM workflow owner.
- CRM should own intent, templates, and business rules; transport stays here.

## Owner

- Engineering (integration team). Architecture: B-Platform architect owns this doc only.
