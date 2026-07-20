---
description: "Use when working with MDFoods Figma designs, UI mockups, design-to-code, code-to-design, or figma.com links in this repository."
applyTo: "products/mdfoods/**,**/*figma*"
---

# MDFoods Figma Design Instructions

Use these instructions when creating, editing, inspecting, syncing, or implementing MDFoods UX/UI designs with Figma tools.

## Source of truth

- Primary Figma file: `https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9/MDFoods.vn`
- Primary file key: `LhJxbmnpQ1D7CY3SIvyCU9`
- Components page node: `117:85`
- MDFoods component and token guide: `products/mdfoods/figma_guide.md`

Before doing any MDFoods design work, read `products/mdfoods/figma_guide.md` and follow its current component inventory, node IDs, color variables, typography rules, button rules, and page composition order.

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

Use `use_figma` for write operations such as creating nodes, cloning components, editing layouts, binding variables, changing text, or validating component usage. Always load the `figma-use` skill guidance before calling `use_figma`.

Use `generate_figma_design` only when importing or capturing an existing web page into an existing Figma design file. For MDFoods page capture, follow the capture workflow in `.github/copilot-instructions.md`.

## Required MDFoods workflow

1. Read `products/mdfoods/figma_guide.md` before tool calls.
2. Use file key `LhJxbmnpQ1D7CY3SIvyCU9` unless the user gives another MDFoods Figma file.
3. Switch to the Components page `117:85` before reading component definitions.
4. Reuse existing components and visual samples before drawing custom nodes.
5. Bind new colors to MDFoods variables where possible instead of hardcoding hex values.
6. Use `Archivo` typography for MDFoods UI text.
7. Validate important updates after writing, including button usage and typography when relevant.

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
