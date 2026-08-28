# api-service-orchestrator

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/api-service-orchestrator` |
| **v3 target** | `api-service-orchestrator` (L2) — inter-service orchestration |
| **Layer** | L2 — API Services |
| **Status** | active (repo created 2026-08-12, NestJS scaffold) |
| **Language** | NestJS 11 + TypeScript (Fastify adapter) |
| **Default branch** | `main` |

Last synced: 2026-08-12

## Purpose

Inter-service orchestration domain service. Owns the **mediation layer** between L2 services: no `api-service-*` calls another `api-service-*` directly — every cross-domain request is saved here first, then dispatched to the target. Owns request routing, response delivery (synchronous / short-poll / long-poll), and per-request lifecycle (timeout, retry, dead-letter). Stays stateless at the routing layer; persistence of payloads and audit logs goes through `dbo-head` like any other L2 service.

## Responsibility

- **Request intake**: accept a typed cross-service request from a caller L2 service (source, target, op, payload, response-pattern hint).
- **Dispatch**: route the request to the target L2 service; never calls `dbo-worker-*` and never touches another L2 service's datastore directly.
- **Response delivery**: choose one of 3 patterns per request — synchronous (final result in the current connection), short polling (1s–15s), or long polling (15s+). Issues a polling URL for the latter two.
- **Lifecycle**: timeout, retry policy, DLQ handoff for failed dispatches.
- **Audit**: record every dispatched request + final status (persisted via `dbo-head`, not in-process).

## Dependencies

- **Callers (L2, all of them)**: every `api-service-*` ([`api-service-ecom`](./api-service-ecom.md), [`api-service-organization`](./api-service-organization.md), [`api-service-social`](./api-service-social.md), [`api-service-integration`](./api-service-integration.md), [`api-service-crm`](./api-service-crm.md), [`api-service-identity`](./api-service-identity.md), [`api-service-content`](./api-service-content.md)) saves cross-domain requests here instead of calling peers directly.
- **Targets (L2, all of them)**: dispatches to whichever L2 service the request names as target. The orchestrator does **not** know target business logic — it only routes.
- **Database Operator (L3)**: [`dbo-head`](./dbo-head.md) — synchronous request/response for persisting request records, audit logs, and DLQ entries. This service does **not** touch the datastore directly.
- **Not consumed by L1**: L1 (cfc/bof) calls business domains directly, never the orchestrator. The orchestrator is L2-internal.

## Public API surface

- **Save request** — `POST /requests` (source, target, op, payload, response-pattern hint) → returns either the final result (synchronous) or a polling URL (short/long).
- **Poll result** — `GET /requests/{id}` (short-polling or long-polling response shape).
- **DLQ** — `GET/POST /requests/{id}/dlq` for inspecting / retrying dead-lettered dispatches.
- Internal dispatch channel (not exposed to L1) — pushes the request to the target L2 service over Redis Streams / HTTP.

## Notes

- ✅ Repo exists on the remote (`github.com/b-platform-vn/api-service-orchestrator`, created 2026-08-12). NestJS 11 scaffold pushed (commit `4e014a4`). Package scope: `@b-platform-vn/api-service-orchestrator`. Consumes `@b-platform-vn/dbo-schemas@^0.1.4` from GitHub Packages.
- ⚠️ Contract drift: remote `bof-web-bplatform/main` currently documents and implements an L1 portal BFF skeleton that calls this service (`apps/portal/src/server/orchestrator-client.ts`), while this repo's architecture contract says it is L2-internal and not consumed by L1. This must be resolved by an ADR before either boundary is treated as the ecosystem-wide rule; declarative UI/action payloads must never choose the service route themselves.
- **Why a service, not a library**: routing + response-pattern negotiation + audit must be a single authoritative boundary, not duplicated into every caller via an SDK. An SDK would let callers bypass the orchestrator under pressure.
- **Why L2, not L0**: it owns business-adjacent state (request lifecycle, DLQ), consumes `dbo-head`, and is consumed only by other L2 services — same layer as the domains it mediates.
- **Local-dev escape hatch**: in local dev, servers can't call back to each other; the synchronous pattern is the default there. In production, default to short/long polling unless the op is provably <1s (e.g. auth checks).

## Owner

- Engineering (platform/core team). Architecture: B-Platform architect owns this doc only.
