# cfc-web-mdfoods

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/cfc-web-mdfoods` (planned) |
| **Current name** | `web-b2b-mdfoods` → [old doc](./deprecated/web-b2b-mdfoods.md) |
| **v3 target** | `cfc-web-mdfoods` (L1 / cfc) — rename opportunistically |
| **Layer** | L1 — User Interaction (cfc) |
| **Status** | planned (rename of `web-b2b-mdfoods`) |
| **Language** | TypeScript (Next.js App Router) |
| **Package** | `web-b2b-mdfoods` `0.0.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Customer-facing **MDFoods B2B storefront**. Largest B-Platform web repo (~962k TS lines). Implements the 20 MDFoods FRDs documented under `products/mdfoods/`.

## Responsibility

- Render all 20 MDFoods features (browse, cart, quote request, quote management, order/payment, order management, registration, auth, profile, addresses, company info, member/permission, in-app messages, password recovery, etc.).
- Calls the MDFoods backend through Next.js Server Actions.

## Dependencies

- **Runtime**: `next`, `react`, `react-dom`, `axios`, `framer-motion`/`motion`, `embla-carousel-*`, `react-datepicker`, `react-paginate`, `react-toastify`/`sonner`, `lucide-react`, `react-icons`, `jwt-decode`, `redis`, `slugify`, `nanoid`, `tailwindcss`.
- **Backend (L2)**: [`api-service-ecom`](./api-service-ecom.md) (B2C parts) + [`api-service-organization`](./api-service-organization.md) (B2B parts) via Next.js Server Actions.
- **E2E**: Playwright (`e2e/`, `playwright.config.ts`).

## Public API surface

- Web UI only.

## Notes

- Local dev: `npm install` → `npm run dev` (port 3000). Scripts: `dev`, `build`, `start`, `lint`, `typecheck`.

## Owner

- Engineering (MDFoods team). Architecture: B-Platform architect owns this doc only.
