# sdk-design-system

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/sdk-design-system](https://github.com/b-platform-vn/sdk-design-system) |
| **v3 target** | → fold into `sdk-platform` (subpath: `/design-system`) |
| **Layer** | SDK |
| **Status** | active |
| **Language** | TypeScript + CSS |
| **Package** | `@b-platform-vn/sdk-design-system` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Design-system React component library for B-Platform. Exported as a Vite + Tailwind bundle (originates from a Figma "Make file" code bundle). Largest SDK (~455k TS). Companion to [`docs-design-system`](./docs-design-system.md).

## Responsibility

- Shared React component primitives (Radix UI + MUI + shadcn-style via `class-variance-authority`/`clsx`/`tailwind-merge`).
- Consumed by B-Platform web frontends.

## Dependencies

- **Runtime**: `@emotion/react`, `@emotion/styled`, `@mui/icons-material`, `@mui/material`, `@popperjs/core`, full `@radix-ui/react-*` suite, `class-variance-authority`, `clsx`, `cmdk`, `date-fns`, `embla-carousel-react`, `input-otp`, `lucide-react`, `motion`, `next-themes`, `react-day-picker`, `react-dnd` + `react-dnd-html5-backend`, `react-popper`, `react-resizable-panels`, `react-responsive-masonry`, `react-router`, `react-slick`, `recharts`, `slick-carousel`, `sonner`, `tailwind-merge`, `tw-animate-css`, `vaul`.
- **Peer**: `react`, `react-dom`.
- **Build**: `vite` + `@vitejs/plugin-react`, `tailwindcss`, `@tailwindcss/vite`, `vite-plugin-dts`.

## Public API surface

- NPM package `@b-platform-vn/sdk-design-system` (GitHub Package Registry).

## Manifest contract

- `vite.config.mts`, `tsconfig.json`, `postcss.config.mjs`, `src/`, `guidelines/`, `ATTRIBUTIONS.md`.
- Original Figma source: https://www.figma.com/design/p9MNO6IcelPrb9E5XttCAF/Design-System-Project-Setup

## Notes

- README is the Figma Make boilerplate. Doc gap: needs a component-library README.

## Owner

- Engineering (design-system team). Architecture: B-Platform architect owns this doc only.
