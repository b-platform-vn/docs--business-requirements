# Copilot Instructions

## Figma Web Page Capture Workflow

Use these instructions whenever capturing a live web page into a Figma design file with the Figma MCP capture tool, or whenever the user asks to use a live website as a design reference/referrer.

### Live website reference-to-design flow

When asked to use a live website as the reference/referrer for a requested design, follow this process:

1. Try to load the referred website in each supported viewport needed for the request:
   - Desktop: `1920x1080`
   - iPad
   - iPhone
2. Capture the page for Design into the current working Figma page.
3. Inspect the captured design and extract reusable component candidates, including layout primitives, cards, form controls, navigation, tables, CTAs, chips, alerts, and state patterns.
4. Consolidate new findings with the available reusable components in the Design Catalogs.
   - Reuse existing catalog components first.
   - If a captured pattern extends an existing component family, adjust the existing component by adding the new finding as another variant instead of creating a separate duplicate family.
   - If no matching component family exists, create a new catalog family from the captured pattern.
5. Create the requested design from Design Catalog components, not directly from the raw capture.

Important catalog rules:

- Use a Figma page as Design Catalogs Storage.
- Keep each component family in one row.
- Represent each component variant as a Frame in that row.
- Every component should have at least two sizing stages: `FILL` and `HUG`.
- Treat the raw captured page as a reference source only; do not leave it as the final componentized design unless the user specifically asks for a raw capture.

### Choose the right browser resolution

Before capturing a screenshot for Figma design, open the browser in simulator mode for the target screen resolution:

- Web/Desktop: `1920x1080`
- iPad
- iPhone / Mobile

By default, use the Web/Desktop resolution. Only switch to Mobile resolution when the user explicitly asks for a mobile capture.

### Prefer the hash-triggered auto-init flow

For external pages such as `mdfoods.vn`, do **not** manually call `window.figma.captureForDesign(...)` after injecting the script. That path can hang indefinitely without error and without sending a network request.

Instead:

1. Generate a fresh Figma capture ID for the target Figma file.
2. Open a clean browser tab.
3. Navigate to the target page with the capture hash already present:
   - `#figmacapture=<captureId>`
   - `figmaendpoint=<url-encoded-submit-url>`
   - `figmadelay=1000`
4. After the page is on the hash URL, inject the capture script:
   - `https://mcp.figma.com/mcp/html-to-design/capture.js`
5. Bring the page to the foreground.
6. Poll the Figma capture status until it reaches `completed`.

The script must see `location.hash` at initialization time so it can auto-trigger. Navigating first and injecting second is the reliable order.

### Use clean state for each capture attempt

- Use a fresh capture ID for every capture attempt.
- Use a fresh browser tab when retrying after a failed or stuck attempt.
- Do not reuse a consumed capture URL; it returns `409 Conflict`.
- Avoid carrying over old injected scripts, init scripts, or route handlers.

### Avoid unnecessary route interception

Only strip CSP headers with Playwright route interception if the target site actually sends a `content-security-policy` header.

Before intercepting all requests, check the target page headers. For pages without CSP, avoid `page.route('**/*', ...)` because intercepting every image, CSS file, and JS chunk can make media-heavy pages time out during navigation.

If route interception was used and is no longer needed, call `page.unrouteAll()` before continuing.

### Keep the capture tab focused

Bring the tab to the foreground before capture and during polling. Background tabs can be throttled by the browser, which may stall capture timers and async serialization.

### Polling expectations

- `pending` means the page is still loading or data has not been submitted yet.
- `processing` means the submit succeeded and conversion is running.
- `completed` means the design was added to Figma.

Keep polling the same `fileKey` and `captureId` every few seconds until completed. Do not generate a new capture ID unless the tab is clearly in a broken state.

### Result quality

Figma capture imports raw frames and text, not linked design-system components. Treat the result as a pixel-perfect reference. If a componentized design is needed, rebuild the screen afterward from the Design Catalogs and design-system components.

## Multica Feature Description Workflow

Use these instructions whenever synchronizing Product Feature Requirement Documents (FRDs) into Multica Feature issues.

### Keep descriptions short and useful

Do **not** paste the full FRD into the Multica issue description. Multica is for task allocation and progress tracking; the full source of truth stays in Git.

Use this concise structure:

```md
## Scope

v2 scope: new implementation.

An **<Feature Name>** feature that <one-sentence outcome / integration summary>.

## References

- Feature Requirement Document: <GitHub URL to the FRD>
- Figma Design: <Figma URL or TBD>

## Notes

- Source: current `products/<product>` specs.
- Feature priority: <P0/P1/P2>
```

### Description rules

- Keep the description under ~600 characters when practical.
- Include only scope, links, and priority.
- Link to the FRD instead of duplicating requirements.
- Include Figma link when the FRD has one; otherwise use `TBD`.
- Mention integration context only when it changes implementation direction.
- Keep issue labels, project, status, and assignee in Multica fields — not repeated in the description.