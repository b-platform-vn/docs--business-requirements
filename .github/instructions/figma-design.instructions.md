---
description: "Use when working with B-Platform Super App or MDFoods Figma designs, UI mockups, design-to-code, code-to-design, or figma.com links in this repository."
applyTo: "products/bplatform-general/**,products/mdfoods/**,**/*figma*"
---

# B-Platform and MDFoods Figma Design Instructions

Use these instructions when creating, editing, inspecting, syncing, or implementing B-Platform Super App or MDFoods UX/UI designs with Figma tools.

## Source of truth

- MDFoods Figma file: `https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9/MDFoods.vn`
- MDFoods file key: `LhJxbmnpQ1D7CY3SIvyCU9`
- MDFoods components page node: `117:85`
- MDFoods component and token guide: `products/mdfoods/figma_guide.md`
- B-Platform Super App Figma file: `https://www.figma.com/design/uIeicyHJ4gCqCe0SdRTaby/B-Platform-Designs`
- B-Platform file key: `uIeicyHJ4gCqCe0SdRTaby`
- B-Platform guide: `products/bplatform-general/figma_guide.md`
- B-Platform full component design catalog: `products/bplatform-general/design-catalog.md`
- B-Platform reference docs: `products/bplatform-general/README.md` and `products/bplatform-general/features/sign-in.md`
- **B-Platform Designs is a full clone of the Sneat Vuetify NuxtJS Admin Template** (`https://demos.themeselection.com/sneat-vuetify-nuxtjs-admin-template/demo-1`, e.g. its [CRM dashboard](https://demos.themeselection.com/sneat-vuetify-nuxtjs-admin-template/demo-1/dashboards/crm)) — reproduce the live template's layout, components, and behavior exactly; do not treat it as loose inspiration.
- **Supported device viewports for B-Platform screens: Mobile, iPad, and Web** — design all three per screen/feature unless the user explicitly scopes a screen as desktop-only. See `products/bplatform-general/figma_guide.md` for exact widths and responsive behavior per viewport.

Before doing any B-Platform design work, read `products/bplatform-general/figma_guide.md` and `products/bplatform-general/design-catalog.md`, and follow their shared shell, navigation, search, color, typography, button, input, table, and sign-in patterns.

## When to use Figma tools

Use Figma tools whenever the user asks to:

- Create or update a screen, component, page, mockup, flow, diagram, visual, or UI spec.
- Inspect a Figma design and implement it in code.
- Push an existing web page or code UI into Figma.
- Sync a design with project documentation or feature requirements.
- Work with a `figma.com` URL.

Use `get_design_context` as the primary read tool for design-to-code tasks when a node-specific Figma URL or node ID is available. Treat generated code as reference only; adapt it to this repository's stack and conventions.

Use `get_metadata` only for lightweight structure discovery when the exact node is unknown. Prefer `get_design_context` once the relevant node is identified.

Use `get_screenshot` when visual validation or a compact image reference is needed.

Use `use_figma` for write operations such as creating nodes, cloning components, editing layouts, binding variables, changing text, updating Design Catalogs, creating component family rows, adding component variants, or validating component usage. Always load the `figma-use` skill guidance before calling `use_figma`.

Use `generate_figma_design` only when importing or capturing an existing web page into an existing Figma design file. For MDFoods page capture, follow the capture workflow in `.github/copilot-instructions.md`.

## Live website reference workflow

When the user asks to use a live website as the design reference/referrer, treat that as an explicit request to capture, inspect, and componentize the reference before creating the requested design.

1. Try to load the referred website in supported viewports for the requested scope:
	- Desktop: `1920x1080`
	- iPad
	- iPhone / Mobile
2. Capture the live page for Design into the current working Figma page.
3. Inspect the captured design and extract reusable component candidates, including layout primitives, cards, tables, forms, inputs, buttons, navigation, chips, alerts, empty states, and responsive/state patterns.
4. Consolidate extracted findings with available reusable components in the Design Catalogs:
	- Reuse existing Design Catalog components first.
	- If a finding belongs to an existing component family, add it as another variant instead of creating a duplicate family.
	- If no suitable family exists, create a new component family row in the Design Catalogs Storage page.
5. Create the requested design from Design Catalog components and variants, not directly from the raw capture.

### Design Catalogs storage rules

- Use a Figma page as Design Catalogs Storage.
- Keep each component family in one row.
- Represent each component variant as a Frame in that row.
- Every component should have at least two sizing stages: `FILL` and `HUG`.
- Keep captured raw pages as references only; componentized output should come from cataloged reusable parts.

## Required MDFoods workflow

1. Read `products/mdfoods/figma_guide.md` before tool calls.
2. Use file key `LhJxbmnpQ1D7CY3SIvyCU9` unless the user gives another MDFoods Figma file.
3. Switch to the Components page `117:85` before reading component definitions.
4. Reuse existing components and visual samples before drawing custom nodes.
5. Bind new colors to MDFoods variables where possible instead of hardcoding hex values.
6. Use `Archivo` typography for MDFoods UI text.
7. Validate important updates after writing, including button usage and typography when relevant.

## Required design quality loop

Before considering any MDFoods design work complete, run this three-step loop until all steps pass:

### Step 1 — Create design from requirements

- Create a design that fits the stated product, business, and user requirements.
- Make the data requirements explicit, especially what information the UI needs to collect from the user.
- Ensure required user inputs, optional inputs, validation states, helper text, and submission outcomes are represented when relevant.
- If the requirements are ambiguous, make safe assumptions visible and design the minimum clear flow needed to satisfy the current scope.

### Step 2 — Validate visual design

- Validate that the correct MDFoods layout, components, page composition, and visual samples are used.
- Ensure UI elements do not overlap, break, clip unexpectedly, or create unusable spacing.
- Ensure colors follow the MDFoods color-variable and palette guidance.
- Ensure typography follows the `Archivo` typography rules from `products/mdfoods/figma_guide.md`.
- Ensure buttons, inputs, containers, and responsive layouts follow the component reuse rules.
- If any visual-design issue is found, fix it and validate again.
- Repeat this step until no visual-design errors are found.

### Step 3 — Validate UX clarity

- Validate that a user with no prior knowledge of the product can still guess how to use the screen or flow.
- Ensure labels, helper text, empty states, validation messages, CTAs, navigation paths, and success/error states make the intended action obvious.
- Ensure the design communicates what happens next after the user submits or completes an action.
- If the UX is unclear, return to Step 1, rework the solution, and repeat the full loop.
- Only stop when Step 1, Step 2, and Step 3 all pass.

## Component reuse rules

- Use `Header`, `Certifications`, `Footer`, `Product Card`, and `Input` from the component inventory in `figma_guide.md`.
- For buttons, do not use the deprecated reusable Button component set. Clone visual button samples listed in `figma_guide.md` instead.
- For forms, prefer cloning the documented `Container` form sample and then editing text/layout as needed.
- Keep containers, inputs, and buttons on **Hug contents** or **Fill container** sizing unless a fixed atomic control size is intentional.
- Use the standard MDFoods desktop page width of `1536` unless the user explicitly requests another target size.

## Standard page composition

For standard MDFoods web screens, compose pages in this order:

1. `Header / Guest` or `Header / Authenticated`
2. Page-specific main content
3. `Certifications`
4. `Footer`

## Tool-call expectations

When using Figma tools:

- Extract `fileKey` and `nodeId` from Figma URLs. Convert `node-id=1-2` to `1:2`.
- Do not guess missing node IDs. If a read operation needs a specific node and the URL lacks one, ask for a node-specific URL or use metadata discovery.
- Load required fonts before changing text nodes.
- Keep mutations small and incremental.
- Return created or mutated node IDs from `use_figma` scripts.
- Do not introduce new reusable components or variants unless the user asks or confirms.

## Validation snippets

Use the validation snippets from `products/mdfoods/figma_guide.md` when relevant:

- Typography validation: ensure final MDFoods screen text uses `Archivo`.
- Button validation: ensure no live instance references the deprecated Button component set.
- Component checks: confirm cloned instances come from documented MDFoods component nodes.

## Reporting back

When reporting design work, include:

- Figma file or node updated.
- Components or samples reused.
- Any created or modified node IDs.
- Validations performed.
- Any remaining manual follow-up needed in Figma.
