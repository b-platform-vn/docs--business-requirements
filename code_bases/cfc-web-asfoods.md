# cfc-web-asfoods

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/cfc-web-asfoods` (planned) |
| **Current name** | `web-b2c-asfoods` → [old doc](./deprecated/web-b2c-asfoods.md) |
| **v3 target** | `cfc-web-asfoods` (L1 / cfc) — rename opportunistically |
| **Layer** | L1 — User Interaction (cfc) |
| **Status** | planned (rename of `web-b2c-asfoods`) |
| **Language** | TypeScript (Next.js App Router + Tailwind) |
| **Package** | `asfoods-landing-react` `0.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Public-facing **AS Foods** website. Bilingual (`/vi` Vietnamese, `/en` English) Next.js storefront. Repo description: "B-Platform Web - AS Foods".

## Responsibility

- Public marketing + content storefront for AS Foods division.
- Reads content from the shared B-Platform content API (`API_BASE_URL`) and selects content division via `ID_KHOI`.

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `@headlessui/react`, `axios`, `framer-motion`, `lucide-react`, `react-icons`, `react-paginate`, `react-scroll`, `swiper`.
- **Backend (L2)**: shared content API — likely [`api-service-content`](./api-service-content.md) (TBD via `API_BASE_URL`).

## Public API surface

- Web UI only.

## Notes

- CI: GitHub Actions publishes staging image on push to `main`, production image on GitHub Release.
- ⚠️ `package.json` deps include `install` and `npm` as deps — likely accidental; flag for cleanup.

## Owner

- Engineering (AS Foods team). Architecture: B-Platform architect owns this doc only.
