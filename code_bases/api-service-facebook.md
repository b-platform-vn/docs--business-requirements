# api-service-facebook

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-facebook` |
| **v3 target** | `api-service-facebook` (L2) — Facebook integration connector |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-09-02

## Purpose

Facebook integration connector service. Owns Facebook Page communication for the B-Platform ecosystem, including send/receive message flows, page webhooks, and delivery tracking.

## Responsibility

- **Facebook Page messaging** — send and receive messages for managed Facebook Pages.
- **Inbound normalization** — transform Facebook Graph / webhook payloads into internal message structures.
- **Outbound delivery** — send messages to the correct Facebook Page thread or user conversation.
- **Delivery audit** — track who requested the send, what was sent, and the delivery result.

## Dependencies

- **Callers (L2, via orchestrator)**: services that need Facebook communication, primarily `api-service-crm`.
- **Upstream system**: Facebook Graph / Messenger APIs.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (delivery logs, page mapping, message state). This service does **not** touch the datastore directly.

## Public API surface

- HTTP webhook endpoints for Facebook Page inbound events.
- Internal send APIs for Facebook Page message delivery.
- Delivery-status query endpoints (shape TBD).

## Notes

- This service is the Facebook connector boundary, not the CRM workflow owner.
- CRM should own customer intent and templates; Facebook transport stays here.

## Owner

- Engineering (integration team). Architecture: B-Platform architect owns this doc only.
