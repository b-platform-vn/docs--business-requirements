# cfc-web-lfarm

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/cfc-web-lfarm` (planned) |
| **Current name** | `web-b2c-lfarm` → [old doc](./deprecated/web-b2c-lfarm.md) |
| **v3 target** | `cfc-web-lfarm` (L1 / cfc) — rename opportunistically |
| **Layer** | L1 — User Interaction (cfc) |
| **Status** | planned (rename of `web-b2c-lfarm`) |
| **Language** | TypeScript (Next.js App Router + Tailwind) |
| **Package** | `lfarm-landing-react` `0.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Public **LFarm** storefront. Next.js + Tailwind. Reads content from shared B-Platform content APIs.

## Responsibility

- Public LFarm landing/storefront.

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `@headlessui/react`, `axios`, `framer-motion`, `lucide-react`, `react-icons`, `react-paginate`, `react-scroll`, `swiper`.
- **Backend (L2)**: content API via `API_BASE_URL` (TBD).

## Public API surface

- Web UI only.

## Notes

- ⚠️ README still shows Vite template text — not product-specific. Doc divergence to flag.
- ⚠️ `package.json` deps include `install` and `npm` as deps — likely accidental; flag for cleanup.

## Owner

- Engineering (LFarm team). Architecture: B-Platform architect owns this doc only.
