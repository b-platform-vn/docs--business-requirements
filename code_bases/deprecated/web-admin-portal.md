# web-admin-portal

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/web-admin-portal](https://github.com/b-platform-vn/web-admin-portal) |
| **v3 target** | `bof-web-bplatform` (L1 / bof) — rename opportunistically |
| **Layer** | Web frontend |
| **Status** | active |
| **Language** | TypeScript (Next.js App Router) |
| **Package** | `web` `0.1.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Next.js admin portal for the B-Platform backoffice. Bootstrapped with `create-next-app`; serves the `admin.b-platform.vn` domain routed via `platform-endpoints` (with 24h WebSocket timeout for admin sessions).

## Responsibility

- Backoffice UI for B-Platform operators.
- Consumes B-Platform admin APIs.

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `@tanstack/react-table`, `axios`, `react-icons`.
- **Backend**: routed through `platform-endpoints` → admin API (TBD which `api-*`).

## Public API surface

- Web UI only — no public package export.

## Notes

- README is the default `create-next-app` boilerplate; no product-specific README yet. Verify actual feature scope against `src/` before extending docs.

## Owner

- Engineering (routed via Multica). Architecture: B-Platform architect owns this doc only.
