# docs-design-system

| | |
|---|---|
| **Repo** | [github.com/b-platform-vn/docs-design-system](https://github.com/b-platform-vn/docs-design-system) |
| **v3 target** | → fold into `docs-platform` |
| **Layer** | Documentation (design) |
| **Status** | active |
| **Language** | TypeScript + CSS + JavaScript + HTML |
| **Package** | `@figma/my-make-file` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Design-system documentation site. A Figma "Make file" code bundle (shadcn/Radix + Tailwind) exported as a reference for design-system work. Pairs with [`sdk-design-system`](./sdk-design-system.md).

## Responsibility

- Document and prototype the B-Platform design system.
- Reference for component variants, theming (`default_shadcn_theme.css`), and guidelines.

## Dependencies

- **Runtime**: same Radix/MUI/shadcn stack as `sdk-design-system` + `canvas-confetti`.
- **Build**: pnpm workspaces (`pnpm-workspace.yaml`, `pnpm-lock.yaml`), Vite, Tailwind.
- **Original Figma**: https://www.figma.com/design/jo1qppH0X4Xe0XLCRZpuk4/Prototype-System-App

## Public API surface

- Documentation site (internal).

## Manifest contract

- `src/`, `guidelines/`, `default_shadcn_theme.css`, `index.html`, `vite.config.ts`, `postcss.config.mjs`, `pnpm-workspace.yaml`, `ATTRIBUTIONS.md`.

## Owner

- Engineering (design-system team). Architecture: B-Platform architect owns this doc only.
