# api-service-whatsapp

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-whatsapp` |
| **v3 target** | `api-service-whatsapp` (L2) — WhatsApp integration connector |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-09-02

## Purpose

WhatsApp integration connector service. Owns WhatsApp Business communication for the B-Platform ecosystem.

## Responsibility

- **Outbound WhatsApp** — send messages for CRM, notifications, and other approved workflows.
- **Inbound WhatsApp** — receive WhatsApp Business webhooks and normalize them into internal message structures.
- **Delivery audit** — track who requested the send, what was sent, and the delivery result.
- **Message state** — maintain delivery, retry, and deduplication state.

## Dependencies

- **Callers (L2, via orchestrator)**: services that need WhatsApp communication, primarily `api-service-crm`.
- **Upstream system**: WhatsApp Business APIs.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (delivery logs, inbox state, message state). This service does **not** touch the datastore directly.

## Public API surface

- HTTP webhook endpoints for inbound WhatsApp events.
- Internal send APIs for outbound WhatsApp delivery.
- Delivery-status query endpoints (shape TBD).

## Notes

- This service is the WhatsApp connector boundary, not the CRM workflow owner.
- CRM should own intent, templates, and business rules; transport stays here.

## Owner

- Engineering (integration team). Architecture: B-Platform architect owns this doc only.
