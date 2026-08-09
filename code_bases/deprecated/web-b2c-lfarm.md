# web-b2c-lfarm

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/web-b2c-lfarm](https://github.com/b-platform-vn/web-b2c-lfarm) |
| **v3 target** | `cfc-web-lfarm` (L1 / cfc) — rename opportunistically |
| **Layer** | Web frontend |
| **Status** | active |
| **Language** | TypeScript (Next.js App Router + Tailwind) |
| **Package** | `lfarm-landing-react` `0.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Public **LFarm** storefront. Next.js + Tailwind. README is the Vite template boilerplate; verify actual framework against `next.config.ts` (repo uses Next.js per the `next.config.ts` + `package.json`).

## Responsibility

- Public LFarm landing/storefront.
- Reads content from shared B-Platform content APIs (per AS Foods README pattern).

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `@headlessui/react`, `axios`, `framer-motion`, `lucide-react`, `react-icons`, `react-paginate`, `react-scroll`, `swiper`.
- **Backend**: content API via `API_BASE_URL` (TBD).

## Public API surface

- Web UI only.

## Notes

- ⚠️ README still shows Vite template text — not product-specific. Doc divergence to flag.
- ⚠️ `package.json` deps include `install` and `npm` as deps — likely accidental; flag for cleanup.

## Owner

- Engineering (LFarm team). Architecture: B-Platform architect owns this doc only.
