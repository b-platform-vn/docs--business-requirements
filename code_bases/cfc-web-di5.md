# cfc-web-di5

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/cfc-web-di5` (planned) |
| **Current name** | `web-b2c-di5` → [old doc](./deprecated/web-b2c-di5.md) |
| **v3 target** | `cfc-web-di5` (L1 / cfc) — rename opportunistically |
| **Layer** | L1 — User Interaction (cfc) |
| **Status** | planned (rename of `web-b2c-di5`) |
| **Language** | TypeScript (Next.js App Router + Tailwind) |
| **Package** | `web-b2c-di5` `0.1.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

New frontend for the **Di5 Kitchen** website. Built with Next.js App Router; README is in Vietnamese.

## Responsibility

- Public Di5 storefront.
- Reads API via `API_BASE_URL` from `.env.local` (e.g. `http://localhost:9000/api`).

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `@headlessui/react`, `axios`, `framer-motion`, `lucide-react`, `react-icons`, `react-paginate`, `react-scroll`, `swiper`.
- **Backend (L2)**: content/store API at `API_BASE_URL` (port 9000 suggests the legacy store API, now folded into [`api-service-ecom`](./api-service-ecom.md)).

## Structure

- `src/services` — API call layer.
- `src/types` — TypeScript types.
- `src/components` — UI components and sections.
- `src/lib` — utilities.

## Public API surface

- Web UI only.

## Owner

- Engineering (Di5 team). Architecture: B-Platform architect owns this doc only.
