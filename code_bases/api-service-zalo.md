# api-service-zalo

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-zalo` |
| **v3 target** | `api-service-zalo` (L2) — Zalo integration connector |
| **Layer** | L2 — API Services |
| **Status** | planned (new) |
| **Language** | TBD (expected NestJS + TypeScript) |
| **Default branch** | `main` |

Last synced: 2026-09-02

## Purpose

Zalo integration connector service. Owns Zalo communication for the B-Platform ecosystem, including Zalo Personal Account and Zalo Official Account send/receive flows.

## Responsibility

- **Zalo Personal Account** — send and receive messages for personal-account-based communication where allowed.
- **Zalo Official Account** — send and receive OA messages, webhook events, and delivery updates.
- **Inbound normalization** — transform Zalo payloads into internal message structures.
- **Outbound delivery** — send platform messages to the correct Zalo channel/account type.
- **Delivery audit** — track who requested the send, what was sent, and the delivery result.

## Dependencies

- **Callers (L2, via orchestrator)**: services that need Zalo communication, primarily `api-service-crm`.
- **Upstream system**: Zalo APIs for Personal Account and Official Account.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for all datastore access (delivery logs, channel mapping, message state). This service does **not** touch the datastore directly.

## Public API surface

- HTTP webhook endpoints for Zalo inbound events.
- Internal send APIs for Zalo Personal Account / Official Account message delivery.
- Delivery-status query endpoints (shape TBD).

## Notes

- This service is the Zalo connector boundary, not the CRM workflow owner.
- CRM should own customer intent and templates; Zalo transport stays here.

## Owner

- Engineering (integration team). Architecture: B-Platform architect owns this doc only.
