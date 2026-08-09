# B-Platform Super App Figma Guide

This guide documents the shared B-Platform Super App design patterns, Figma workflow expectations, and visual tokens for working with B-Platform Super App screens and components.

- Figma file: [B-Platform Designs](https://www.figma.com/design/uIeicyHJ4gCqCe0SdRTaby/B-Platform-Designs)
- File key: `uIeicyHJ4gCqCe0SdRTaby`
- Sign-in flow reference: [B-Platform Designs sign-in sample](https://www.figma.com/design/uIeicyHJ4gCqCe0SdRTaby/B-Platform-Designs?node-id=99-5674&t=iJnLOB71NoIQYyFq-4) — **unverified as of 2026-08-07; re-check with `get_metadata` before reuse, node IDs may be stale**
- Reference docs: `products/bplatform-general/README.md`, `products/bplatform-general/features/sign-in.md`
- **Full component design catalog**: `products/bplatform-general/design-catalog.md`
- **Source-of-truth reference product to clone**: [Sneat Vuetify NuxtJS Admin Template, Demo 1](https://demos.themeselection.com/sneat-vuetify-nuxtjs-admin-template/demo-1), e.g. [CRM dashboard](https://demos.themeselection.com/sneat-vuetify-nuxtjs-admin-template/demo-1/dashboards/crm)

## Purpose

Use this guide when creating or updating B-Platform Super App Figma work, especially shared shell screens, global navigation, admin pages, sign-in flows, and platform-level UX patterns.

## B-Platform Design is a full clone of the Sneat Admin Template

**B-Platform Designs is being CLONED, page-for-page and component-for-component, from the Sneat Vuetify NuxtJS Admin Template** (`demos.themeselection.com/sneat-vuetify-nuxtjs-admin-template/demo-1`). This is not a loose inspiration reference or a token-only borrow — treat the live template as the exact source of truth for layout, structure, spacing, component variants, and visual style across the whole Super App, not just the shell/nav/sign-in patterns already documented.

- When building any B-Platform screen, first find the closest matching route in the live Sneat demo (see `design-catalog.md` section 9, "Sample pages by pattern") and reproduce it as closely as possible before adapting content to B-Platform's domain.
- If a needed page/pattern isn't yet deep-captured in `design-catalog.md` (see its "Open gaps" section), inspect the live demo page directly (computed styles, screenshot) rather than guessing.
- Deviations from the template must be intentional and content-driven (B-Platform terminology, data, navigation entries) — not stylistic reinterpretation.

### Supported device viewports

Every B-Platform Super App screen must be designed for **three** viewports, matching the responsive behavior already verified in `design-catalog.md` section 8 ("Layout principles"):

| Viewport | Approx. width | Notes |
|---|---:|---|
| Web / Desktop | `1920px` (design frame), `1536px` (content reference width used in the crawl) | Full sidebar (`260px`) + fluid content area |
| iPad / Tablet | `~1024px` | Sidebar stays fixed width; content column scales down proportionally |
| Mobile | `~375–430px` | Sidebar collapses/hides behind a toggle per the template's mobile nav pattern |

When creating a new screen or feature row in Figma, produce frames for all three viewports (Web, iPad, Mobile) unless the user explicitly says a screen is desktop-only. Place same-feature viewport variants together in that feature's row, following the "Rows by feature" canvas rule below — order Web → iPad → Mobile left-to-right.

Before starting detailed design work, read `products/bplatform-general/design-catalog.md`. It documents the full component-level design system (buttons, inputs, typography, containers, tables, colors, layout principles, and sample page mappings) extracted from a systematic crawl of all ~112 pages in the Sneat Vuetify NuxtJS Admin Template reference. Use it as the exact-reproduction source for token values instead of re-deriving styles from scratch.

## When to use Figma tools

Use Figma tools for B-Platform Super App work when the user asks to:

- Create or update a B-Platform Super App screen, shell layout, overlay, or component.
- Inspect existing B-Platform design nodes or component usage in Figma.
- Push a live B-Platform page or admin shell into Figma.
- Sync B-Platform design changes with architecture or feature documentation.
- Work with a `figma.com` URL that targets the B-Platform Figma file.

## Design system foundations

B-Platform Super App designs should reuse shared shell and admin patterns.

- Shell layout: left navigation/sidebar, top header, main page content.
- Keep global navigation consistent across screens.
- Use compact admin spacing with `24px` page padding and `16px` section gaps.
- Prefer white surface cards, subtle grey backgrounds, and clear hierarchy.
- Use page content width and layout that match the shell pattern in the reference file.

### Core visual tokens

These tokens are derived from the Sneat admin reference used as the starting design system vocabulary. See `products/bplatform-general/design-catalog.md` for the complete color scale, typography scale, button/input variant matrices, table styles, and layout spacing rules.

- Primary: `#696CFF`
- On primary: `#FFFFFF`
- Background: `#F5F5F9`
- Surface: `#FFFFFF`
- Surface variant: `#424242`
- Grey 50: `#FAFAFA`
- Grey 100: `#F5F5F5`
- Grey 200: `#EEEEEE`
- Grey 300: `#E0E0E0`
- Grey 400: `#BDBDBD`
- Grey 500: `#9E9E9E`
- Grey 600: `#757575`
- Grey 700: `#616161`
- Grey 800: `#424242`
- Grey 900: `#212121`
- Warning: `#FFAB00`
- Error: `#FF3E1D`
- Info: `#03C3EC`
- Success: `#71DD37`

### Typography and text

- The reference admin template uses `Public Sans` for body text and UI labels.
- Use the same font family when matching the shared B-Platform shell style.
- Preserve text hierarchy and weight consistency for headers, labels, and body text.
- When editing text inside a Figma file, always load the required font before mutating the node.

## Component reuse rules

- Prefer shared shell components and patterns over new ad-hoc layouts.
- Reuse existing button, input, card, and navigation samples from the reference file when possible.
- Keep controls on `Hug contents` or `Fill container` sizing unless a fixed component width is intentional.
- For button visuals, clone an existing sample rather than creating a new one from scratch.
- Match component variants (button variant, input variant, table density) to the mappings in `products/bplatform-general/design-catalog.md` section "Sample pages by pattern" when starting a new B-Platform screen.

## Layout rules

- Left navigation width should remain consistent with the reference shell.
- Maintain `24px` horizontal page padding inside the main content area.
- Use `16px` vertical spacing between sections and `24px` between major page blocks.
- Cards should use a white fill, `6px` corner radius, and a subtle border such as `rgba(34, 48, 62, 0.12)`.
- Design every screen across the three supported viewports — **Web (1920px frame / 1536px content reference), iPad (~1024px), Mobile (~375–430px)** — reproducing the reference template's responsive behavior (fixed sidebar width on tablet, collapsing/hidden sidebar on mobile, proportionally scaled content columns) rather than inventing new breakpoint behavior.

## Figma canvas organization

Organize B-Platform Figma work at two levels: **Figma pages by module**, and **canvas rows by feature**.

### Pages by module

- Give each module (e.g. `UniGate`, `CRM`, `General` / shared shell, `MDFoods`, future products) its own Figma page in the file.
- Do not mix screens from different modules on the same Figma page.
- Name each page after its module so the Pages panel stays navigable as the file grows.
- When starting work on a module that doesn't have a page yet, create one before adding screens.

### Rows by feature

Within a module's page, organize screen frames by **feature**, not by creation order.

- Group every screen that belongs to the same feature (e.g. `Sign-in`, `Global Search`, `User List`, `Roles & Permissions`) into a single horizontal row on the canvas.
- Lay out frames left-to-right within a feature's row, generally following the user flow order (entry screen first, then subsequent steps/states).
- When a feature has multiple supported-viewport variants (Web, iPad, Mobile), group each screen's viewport variants together (e.g. `Sign-in / Web`, `Sign-in / iPad`, `Sign-in / Mobile`) before moving to the next step/state in the flow, ordered Web → iPad → Mobile.
- Keep vertical space between feature rows so each row is visually distinct and easy to scan top-to-bottom.
- When adding a new screen to an existing feature, place it into that feature's row instead of creating a new row or appending it elsewhere on the page.
- When starting a new feature, create a new row rather than mixing its screens into another feature's row.
- Name each row/section (e.g. via a Figma Section) after the feature it contains.
- If a feature's screens already exist but are scattered across the page, ask before mass-relocating nodes; moving many frames is a large mutation and should be confirmed first.

## Figma workflow

- Use `get_design_context` as the primary read tool for node-specific inspection.
- Use `use_figma` for write operations such as cloning components, editing text, and updating layouts.
- Use `generate_figma_design` only when importing an actual live B-Platform page into an existing Figma file.
- Do not guess missing node IDs. If a read or write operation needs a specific node and the URL lacks one, ask for a node-specific URL or use metadata discovery.

## Validation checklist

Before considering B-Platform Figma work complete, validate:

- The shared shell layout follows the left navigation / top header / content pattern.
- Colors use the reference token palette instead of hardcoded values.
- Typography and spacing match the Super App reference.
- Buttons, inputs, and cards reuse existing samples when possible.
- Page composition and navigation patterns follow `products/bplatform-general/README.md`.
- New or moved screens live on their module's Figma page and are placed in their feature's row, not scattered or mixed with another module's or feature's screens.
- The screen/feature has Web, iPad, and Mobile variants (unless explicitly scoped as desktop-only), and each reproduces the live Sneat template's responsive behavior for that route rather than an invented layout.

## Reporting back

When reporting B-Platform Figma work, include:

- The Figma file key and node IDs updated.
- Components or samples reused.
- Any new nodes created.
- Validation steps performed.
- Any design decisions that should be reviewed manually.
