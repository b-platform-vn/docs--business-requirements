# web-b2b-mdfoods

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/web-b2b-mdfoods](https://github.com/b-platform-vn/web-b2b-mdfoods) |
| **v3 target** | `cfc-web-mdfoods` (L1 / cfc) — rename opportunistically |
| **Layer** | Web frontend |
| **Status** | active |
| **Language** | TypeScript (Next.js App Router) |
| **Package** | `web-b2b-mdfoods` `0.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Standalone Next.js web application for the **MDFoods B2B** platform — customer-facing B2B storefront for business buyers. Largest B-Platform web repo (~962k TS lines). See `products/mdfoods/` for the FRDs this frontend implements.

## Responsibility

- Render all 20 MDFoods features (browse, cart, quote request, quote management, order/payment, order management, registration, auth, profile, addresses, company info, member/permission, in-app messages, password recovery, etc.).
- Calls the MDFoods backend through Next.js Server Actions.

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `axios`, `framer-motion` / `motion`, `embla-carousel-*`, `react-datepicker`, `react-paginate`, `react-toastify` / `sonner`, `lucide-react`, `react-icons`, `jwt-decode`, `redis`, `slugify`, `nanoid`, `tailwindcss`.
- **Backend**: [`api-b2b-mdfoods`](./api-b2b-mdfoods.md) via Next.js Server Actions (per README).
- **E2E**: Playwright (`e2e/`, `playwright.config.ts`).

## Public API surface

- Web UI only.

## Manifest contract

- Standard Next.js app with App Router (`src/app/`), Redux store under `src/integrations/`, feature modules under `src/features/`.

## Notes

- Local dev: `npm install` → `npm run dev` (port 3000). Scripts: `dev`, `build`, `start`, `lint`, `typecheck`.

## Owner

- Engineering (MDFoods team). Architecture: B-Platform architect owns this doc only.
