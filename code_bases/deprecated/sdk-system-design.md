# sdk-system-design

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/sdk-system-design](https://github.com/b-platform-vn/sdk-system-design) |
| **v3 target** | → fold into `sdk-platform` (subpath: `/system-design`; retire `@bplatform/` scope) |
| **Layer** | SDK |
| **Status** | active |
| **Language** | TypeScript + JavaScript + CSS |
| **Package** | `@bplatform/sdk-system-design` `0.1.0` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Base UI components and layout system for B-Platform **Zalo Mini Apps**. Uses a JSON-render approach (`@json-render/core`, `@json-render/react`) with `zod` validation and `lucide-react` icons.

## Responsibility

- Shared UI/layout primitives for Zalo Mini Apps.
- JSON-render-based component system.

## Dependencies

- **Runtime**: `@json-render/core`, `@json-render/react`, `lucide-react`, `zod`.
- **Peer**: `react`, `react-dom`.
- **Build**: `vite` + `@vitejs/plugin-react`, `vite-plugin-dts`, `tailwindcss`, `postcss`, `autoprefixer`, `typescript`.

## Public API surface

- NPM package `@bplatform/sdk-system-design` (note: older `@bplatform/` scope, not `@b-platform-vn/`).

## Manifest contract

- `src/`, `docs/`, `tailwind.config.js`, `postcss.config.js`, `vite.config.ts`, `tsconfig.json`.

## Notes

- ⚠️ Uses a different package scope (`@bplatform/`) than the rest of the org (`@b-platform-vn/`). Flag for naming-convention alignment.

## Owner

- Engineering (Zalo Mini App team). Architecture: B-Platform architect owns this doc only.
