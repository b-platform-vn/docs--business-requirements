# design-mdfoods-landing

| | |
|---|---|
| **Repo** | `github.com/b-platform-vn/design-mdfoods-landing` (planned) |
| **Current name** | `Mdfoodsdesign` → [old doc](./deprecated/Mdfoodsdesign.md) |
| **v3 target** | retire — design reference bundle no longer tracked as a repo |
| **Layer** | Design source bundle |
| **Status** | planned (rename of `Mdfoodsdesign`) |
| **Language** | TypeScript + CSS + JavaScript + HTML |
| **Package** | `@figma/my-make-file` `0.0.1` |
| **Default branch** | `main` |

Last synced: 2026-08-09

## Purpose

Figma "Make file" code bundle for the **B2B Landing Page Design**. Exported as a starting reference for MDFoods B2B landing-page design work, not a production service. Introduces the `design-` prefix for Figma Make reference bundles.

## Responsibility

- Reference source for the MDFoods B2B landing page UI.

## Dependencies

- **Runtime**: same Radix/MUI/shadcn stack as the `design-system` subpath of [`sdk-platform`](./sdk-platform.md).
- **Build**: Vite + Tailwind.
- **Original Figma**: https://www.figma.com/design/Q4mMmvgpKHlZZuh86sm0GS/B2B-Landing-Page-Design

## Public API surface

- None — reference bundle only.

## Notes

- ⚠️ Current repo name `Mdfoodsdesign` is PascalCase — violates the lowercase convention. Rename to `design-mdfoods-landing` when convenient.

## Owner

- Engineering (design-system team). Architecture: B-Platform architect owns this doc only.
