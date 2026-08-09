# platform-endpoints

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/platform-endpoints](https://github.com/b-platform-vn/platform-endpoints) |
| **v3 target** | retire — nginx routing config removed from code_bases topology |
| **Layer** | Platform infrastructure (DevOps-owned) |
| **Status** | active |
| **Language** | HTML + Dockerfile |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Nginx-based reverse proxy configuration for routing requests to various B-Platform services and endpoints. Single source of truth for public ingress routing.

## Responsibility

Route by `Host` header to backend services:

| Domain | Routes to |
|---|---|
| `odeli.vn.internal` | `$ODELI_VN_ENDPOINT` |
| `b-platform.vn.internal` | `$BPLATFORM_VN_ENDPOINT` |
| `api.b-platform.vn.internal` | `$BPLATFORM_API_ENDPOINT` (catch-all) |
| `api.b-platform.vn.internal` `/webhook/*`, `/connect/*` | `$BPLATFORM_WEBHOOK_ENDPOINT` |
| `admin.b-platform.vn.internal` | `$ADMIN_PORTAL_ENDPOINT` (WebSocket 86400s) |
| `crm.b-platform.vn.internal` | `$BPLATFORM_CRM_ENDPOINT` |
| `chat.b-platform.vn.internal` | `$BPLATFORM_CHAT_ENDPOINT` |
| `cloud_integration.b-platform.vn` | `$BPLATFORM_CLOUD_INTEGRATION_ENDPOINT` |
| `/store-api/*` | `$STORE_API_ENDPOINT` (→ [`api-service-ecom`](./api-service-ecom.md)) |

## Dependencies

- **Runtime**: nginx (`nginx/`, `Dockerfile`, `www/`).
- **Backends**: every public-facing B-Platform service.

## Public API surface

- HTTP ingress (port 80).

## Manifest contract

- `nginx/`, `www/`, `Dockerfile`, `README.md`.

## Owner

- **`[B-Platform] DevOps`**. Architecture: B-Platform architect owns this doc only.
