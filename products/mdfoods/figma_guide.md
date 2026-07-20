# MDFoods Figma UX/UI Guide

This guide documents the pre-built MDFoods UI assets currently available in the Figma file and how agents should reuse them through Figma MCP `use_figma` calls.

- Figma file: [MDFoods.vn](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9/MDFoods.vn)
- Components page node: `117:85`
- Current component source inspected on: 2026-07-20

## Components page inventory

Source page: `117:85` (`Components`). Current top-level assets:

| Asset | Node ID | Type | Size | Notes |
|---|---:|---|---:|---|
| `Header` | `117:169` | Component set | `1576 × 270` | Guest and authenticated header variants |
| `Certifications` | `117:294` | Component | `1536 × 204.8` | Standard certification strip |
| `Footer` | `117:403` | Component | `1536 × 571.95` | Standard footer |
| `Product Card` | `128:4435` | Component | `313.325 × 463.988` | Product listing card |
| `Input` | `130:4730` | Component set | `404.4 × 216.8` | Email empty and invalid input variants |
| `Inputs` | `170:6421` | Frame | `364.4 × 107.2` | Input usage samples |
| `Button` | `157:6041` | Frame | `955.975 × 204` | Visual Button samples; not a reusable component |
| `Typography` | `163:6087` | Frame | `1992 × 3916` | Archivo typography specimen |
| `Colors` | `163:6349` | Frame | `3378 × 2712` | MDFoods palette bound to variables |

## General usage rules

- Always load the `figma-use` guidance before calling `use_figma`.
- Always switch to the Components page before reading component definitions:
  - Page node: `117:85`
- Prefer built component instances over manually drawn frames, except for Buttons.
- Do **not** use the reusable Button component set in feature screens. For Buttons, clone a visual variant sample or reuse an existing detached visual copy. Button clones are easier to customize for one-off labels, widths, icon visibility, and form-specific action layouts.
- If no built component exists for an element, use the available color variables before hardcoding colors.
- While using Container, Input, and Button, always use **Hug contents** or **Fill container** sizing.
- Current color variables:
  - `Primary` — `VariableID:131:4992`
  - `Text Primary` — `VariableID:131:4993`
  - `Text Default` — `VariableID:173:6393`
  - `Default Text` — `VariableID:173:6806`
  - `Text Error` — `VariableID:130:4747`
  - `Border Error` — `VariableID:130:4748`

## Color palette

Primary color source: `#2D7041`.

Use the `Primary/*` scale for primary-color variants and the semantic variables for production UI binding.

Palette documentation frame: `163:6349` (`Colors`) on the Components page. Current frame size is `3378 × 2712`; it contains `25` swatches, with all MDFoods primary swatches bound to local variables.

### Primary scale variables

| Variable | ID | Hex | Recommended use |
|---|---|---:|---|
| `Primary/50` | `VariableID:178:5784` | `#F2F8F4` | Very subtle backgrounds |
| `Primary/100` | `VariableID:178:5785` | `#E4F0E8` | Subtle backgrounds, hover surfaces |
| `Primary/200` | `VariableID:178:5786` | `#C8E0D0` | Soft borders, selected fills |
| `Primary/300` | `VariableID:178:5787` | `#9DC5AA` | Disabled / low-emphasis green |
| `Primary/400` | `VariableID:178:5788` | `#6FA17E` | Secondary accents |
| `Primary/500` | `VariableID:178:5789` | `#2D7041` | Base brand primary |
| `Primary/600` | `VariableID:178:5790` | `#285F39` | Hover state |
| `Primary/700` | `VariableID:178:5791` | `#234E32` | Active / pressed state |
| `Primary/800` | `VariableID:178:5792` | `#1E3F2A` | High-emphasis dark accent |
| `Primary/900` | `VariableID:178:5793` | `#173121` | Dark text/accent on light surfaces when needed |
| `Primary/950` | `VariableID:178:5794` | `#0D1B12` | Deepest primary tone |

### Semantic primary variables

| Variable | ID | Hex | Scope |
|---|---|---:|---|
| `Primary` | `VariableID:131:4992` | `#2D7041` | Frame/shape fill, stroke |
| `Text Primary` | `VariableID:131:4993` | `#2D7041` | Text fill |
| `Background Primary` | `VariableID:178:5795` | `#2D7041` | Frame/shape fill |
| `Border Primary` | `VariableID:178:5796` | `#2D7041` | Stroke |
| `Primary Hover` | `VariableID:178:5797` | `#285F39` | Hover fill/stroke |
| `Primary Active` | `VariableID:178:5798` | `#234E32` | Active/pressed fill/stroke |
| `Primary Subtle` | `VariableID:178:5799` | `#E4F0E8` | Subtle green surface |
| `Primary Surface` | `VariableID:178:5800` | `#F2F8F4` | Lightest green surface |

### Color binding rules

- Bind primary Button fills to `Primary` unless a specific state is required.
- Bind primary Button hover states to `Primary Hover`.
- Bind primary Button active/pressed states to `Primary Active`.
- Bind outlined Button strokes to `Border Primary`.
- Bind green text/icon strokes/fills to `Text Primary`.
- Bind subtle green notices or cards to `Primary Surface` or `Primary Subtle`.
- Do not hardcode `#2D7041` in new Figma nodes when a variable can be bound.

## Common Figma tool-call pattern

Use `mcp_figma_mcp_ser_use_figma` with:

- `fileKey`: `LhJxbmnpQ1D7CY3SIvyCU9`
- `skillNames`: `figma-use`

Minimal read pattern:

```js
const page = await figma.getNodeByIdAsync('117:85');
await figma.setCurrentPageAsync(page);

const component = await figma.getNodeByIdAsync('NODE_ID');
return {
  id: component.id,
  name: component.name,
  type: component.type,
  key: component.key,
};
```

Minimal instance pattern:

```js
const page = await figma.getNodeByIdAsync('117:85');
await figma.setCurrentPageAsync(page);

const component = await figma.getNodeByIdAsync('COMPONENT_NODE_ID');
const instance = component.createInstance();
instance.name = 'Component / Usage Name';
instance.x = 0;
instance.y = 0;

return { createdNodeIds: [instance.id] };
```

> When mutating text inside a component instance or visual copy, load the target text node fonts first with `figma.loadFontAsync(...)`.

## Typography

### Reference

- Typography specimen frame: `163:6087`
- Size: `1992 × 3916`
- Font family: `Archivo`
- Text nodes scanned: `215`
- Local text styles currently found in file: none

### Archivo usage

The current Typography specimen uses only Archivo:

| Style | Segment count |
|---|---:|
| `Archivo Bold` | `112` |
| `Archivo Regular` | `99` |
| `Archivo Medium` | `4` |

Typography specimen sizes currently present:

| Size | Count |
|---:|---:|
| `48px` | `6` |
| `36px` | `6` |
| `28px` | `6` |
| `22px` | `6` |
| `20px` | `7` |
| `16px` | `87` |
| `14px` | `89` |
| `12px` | `8` |

### Typography rules

- Use `Archivo` for MDFoods UI text.
- Preserve the typography specimen hierarchy when creating new screens.
- Load the exact Archivo style before mutating text in Figma.
- Use `Archivo SemiBold`, `14px` for Button labels unless a copied visual sample already defines a different matching label style.

### Typography validation snippet

```js
const page = await figma.getNodeByIdAsync('117:85');
await figma.setCurrentPageAsync(page);

const typography = await figma.getNodeByIdAsync('163:6087');
const texts = typography.findAllWithCriteria({ types: ['TEXT'] });
const nonArchivo = [];

for (const text of texts) {
  for (const segment of text.getStyledTextSegments(['fontName'])) {
    const font = segment.fontName;
    if (font.family !== 'Archivo') {
      nonArchivo.push({ id: text.id, name: text.name, font });
    }
  }
}

return {
  textNodeCount: texts.length,
  nonArchivoCount: nonArchivo.length,
  nonArchivo,
};
```

The expected result is `nonArchivoCount: 0`.

## Buttons

### Reference

- Page scanned for Button-like nodes: `0:1` (`v1.0.0`)
- Visual reference node: `157:6041`
- Deprecated reusable component set node: `131:4870` — do not use in feature screens
- Deprecated component set key: `df6f95cc70ee1220f5324fa84b9a12adbe32c5f5`
- Visual reference size: `281.325 × 44` or `281.325 × 46` depending on variant
- Default label text: `Add to quote`
- Font: `Archivo SemiBold`, `14px`

### Button usage strategy

Use a **copy of a visual variant sample** as the default Button workflow.

Why:

- Visual samples are regular editable frames after cloning.
- They are easier to customize than reusable component instances.
- Width, label, icon visibility, and one-off layout changes can be edited directly.
- This avoids component-detach friction when a flow needs custom button text or sizing.

Never use the reusable component set `131:4870` in feature screens. For FRD flow mockups, UX sketches, and feature-specific screens, clone a visual sample or copy an existing detached visual button.

### Visual variants learned from `157:6041`

| Visual variant | Node ID | Intent | Colors |
|---|---:|---|---|
| `Button / Default - Icon Left` | `157:6009` | Primary filled button with left icon | Background green; text/icon white |
| `Button / Default - No Icons` | `157:6001` | Primary filled button without icon | Background green; text white |
| `Button / Default - Icon Right` | `157:6016` | Primary filled button with right icon | Background green; text/icon white |
| `Button / No Background - Icon Left` | `157:6026` | Text/ghost button with left icon | Text/icon uses `Text Primary` |
| `Button / No Background - No Icons` | `157:6032` | Text/ghost button without icon | Text uses `Text Primary` |
| `Button / No Background - Icon Right` | `157:6034` | Text/ghost button with right icon | Text/icon uses `Text Primary` |
| `Button / Outlined - Icon Left` | `157:6043` | Outlined button with left icon | Border uses `Primary`; text/icon uses `Text Primary` |
| `Button / Outlined - No Icons` | `157:6049` | Outlined button without icon | Border uses `Primary`; text uses `Text Primary` |
| `Button / Outlined - Icon Right` | `157:6051` | Outlined button with right icon | Border uses `Primary`; text/icon uses `Text Primary` |

### Canonical Button visual samples on page `0:1`

The scan of page `0:1` found many in-screen Button nodes. Use these canonical visual samples first because they cover the intended MDFoods button styles in one place:

| Style | Icon position | Sample node ID | Size | Recommended use |
|---|---|---:|---|---|
| Default / filled | Left | `157:6009` | `281.325 × 44` | Primary CTA with icon |
| Default / filled | None | `157:6001` | `281.325 × 44` | Primary CTA without icon |
| Default / filled | Right | `157:6016` | `281.325 × 44` | Primary CTA that indicates forward movement |
| No Background | Left | `157:6026` | `281.325 × 44` | Ghost action with icon |
| No Background | None | `157:6032` | `281.325 × 44` | Secondary text action |
| No Background | Right | `157:6034` | `281.325 × 44` | Ghost action with trailing icon |
| Outlined | Left | `157:6043` | `281.325 × 46` | Secondary bordered action with icon |
| Outlined | None | `157:6049` | `281.325 × 44` | Secondary bordered action |
| Outlined | Right | `157:6051` | `281.325 × 46` | Secondary bordered action with trailing icon |

Additional Button-like nodes found on page `0:1` include page-specific copies such as:

| Example node | Type | Text | Use as sample when |
|---:|---|---|---|
| `89:343` | Frame | `APPLY FOR ACCOUNT` | Registration/apply-account page button sizing is needed |
| `90:225` | Frame wrapper | `SIGN-IN` | Authentication form button spacing is needed |
| `131:4896` | Instance | `SIGN-IN` | Existing sign-in action is needed as a local reference |
| `131:4907` | Instance | `Continue with selected Company` | Company-selection primary action is needed |
| `108:428` | Frame | `Proceed to Quote Review` | Shopping cart / quote CTA width is needed |
| `108:433` | Frame | `Continue Shopping` | Shopping cart secondary action is needed |

Prefer the canonical samples under `157:6041` unless a feature screen needs to match one of these page-specific button sizes exactly.

### Detached Button visual copies currently used

The following previously-used Button component instances were detached into editable visual copies on page `0:1`.

| Visual copy node | Previous node | Text | Context |
|---:|---:|---|---|
| `161:6117` | `144:5974` | `Cancel` | Registration — new business |
| `161:6126` | `144:5980` | `Submit registration` | Registration — new business |
| `161:6135` | `147:5980` | `Back` | Registration — add company |
| `161:6144` | `147:5986` | `Create company pending approval` | Registration — add company |
| `161:6153` | `147:6011` | `Use different Tax ID` | Registration — join existing company |
| `161:6162` | `147:6017` | `Send join request` | Registration — join existing company |
| `161:6171` | `147:6038` | `Clear temporary info` | Registration — quote request recovery |
| `161:6180` | `147:6044` | `Continue Request a Quote` | Registration — quote request recovery |
| `161:6189` | `147:6065` | `Resend OTP` | Registration — Email OTP next version |
| `161:6198` | `147:6071` | `Verify email and submit` | Registration — Email OTP next version |
| `161:6204` | `131:4907` | `Continue with selected Company` | Authentication — company selection |
| `161:6207` | `131:4889` | `SIGN-IN` | Authentication — sign in |
| `161:6210` | `131:4896` | `SIGN-IN` | Authentication — sign in |

### Button tool calls

Clone a visual Button sample:

```js
const page = await figma.getNodeByIdAsync('0:1');
await figma.setCurrentPageAsync(page);

const sample = await figma.getNodeByIdAsync('157:6001'); // Default - No Icons
const button = sample.clone();
button.name = 'Button / Primary / Custom';
button.x = 0;
button.y = 0;

return { createdNodeIds: [button.id] };
```

Clone a no-background button:

```js
const page = await figma.getNodeByIdAsync('0:1');
await figma.setCurrentPageAsync(page);

const sample = await figma.getNodeByIdAsync('157:6032'); // No Background - No Icons
const button = sample.clone();
button.name = 'Button / Secondary / Custom';
button.x = 0;
button.y = 0;

return { createdNodeIds: [button.id] };
```

Clone an outlined button:

```js
const page = await figma.getNodeByIdAsync('0:1');
await figma.setCurrentPageAsync(page);

const sample = await figma.getNodeByIdAsync('157:6049'); // Outlined - No Icons
const button = sample.clone();
button.name = 'Button / Outlined / Custom';
button.x = 0;
button.y = 0;

return { createdNodeIds: [button.id] };
```

Override cloned button label text safely:

```js
const button = await figma.getNodeByIdAsync('BUTTON_CLONE_ID');
const label = button.findAllWithCriteria({ types: ['TEXT'] })[0];

for (const segment of label.getStyledTextSegments(['fontName'])) {
  await figma.loadFontAsync(segment.fontName);
}

label.characters = 'Submit registration';
return { mutatedNodeIds: [label.id] };
```

Resize a cloned button and keep the label centered:

```js
const button = await figma.getNodeByIdAsync('BUTTON_CLONE_ID');
button.resize(372, 44);

const label = button.findAllWithCriteria({ types: ['TEXT'] })[0];
if (label) {
  for (const segment of label.getStyledTextSegments(['fontName'])) {
    await figma.loadFontAsync(segment.fontName);
  }
  label.textAlignHorizontal = 'CENTER';
  label.resize(button.width, label.height);
  label.x = 0;
}

return { mutatedNodeIds: [button.id, label?.id].filter(Boolean) };
```

For custom manual outlined buttons, bind:

- Stroke color to `Primary` (`VariableID:131:4992`)
- Text/icon color to `Text Primary` (`VariableID:131:4993`)

Ask for confirmation before introducing a new reusable Button component or component variant.

### Button validation rule

After creating or updating MDFoods screens, verify that no live instance references the deprecated Button component set:

```js
const page = await figma.getNodeByIdAsync('0:1');
await figma.setCurrentPageAsync(page);

const deprecatedButtonSetKey = 'df6f95cc70ee1220f5324fa84b9a12adbe32c5f5';
const usages = page.findAllWithCriteria({ types: ['INSTANCE'] }).filter((instance) => {
  const main = instance.mainComponent;
  const set = main?.parent?.type === 'COMPONENT_SET' ? main.parent : null;
  return set?.key === deprecatedButtonSetKey;
});

return {
  deprecatedButtonUsageCount: usages.length,
  deprecatedButtonUsageIds: usages.map((node) => node.id),
};
```

The expected result is `deprecatedButtonUsageCount: 0`.

## Input

### Reference

- Component set node: `130:4730`
- Component set key: `1a0d609529433ef07f46733e734e8b2c70b65771`
- Sample frame node: `170:6421`
- Component set size: `404.4 × 216.8`
- Variant size: `364.4 × 45.6`
- Font: `Archivo Regular`, `14px`

### Variants

| Variant | Node ID | Component key | Size | Visual state |
|---|---:|---|---:|---|
| `Property 1=Email - Empty` | `130:4729` | `b748be945a175a2897f96c65a1a3e76cfcc86c91` | `364.4 × 45.6` | Fill `#F9FAFB`, border `#E5E7EB` |
| `Property 1=Email - Invalid Input` | `130:4733` | `e10cc3ca476eb846b4430a1a768fd2b36766c1c2` | `364.4 × 45.6` | Fill `#F9FAFB`, border uses error red `#D3242C` |

### Recommended use

Use the Input component set for email-like text inputs, form fields, and validation examples when the current visual language is sufficient. For unsupported field types, duplicate the closest Input variant and edit the label/value text after loading Archivo fonts.

When placing Input in a form container:

- Use **Fill container** for the Input width when the parent form controls sizing.
- Use **Hug contents** for internal content rows or labels.
- Keep Input height fixed unless the design intentionally supports multiline content.
- Bind invalid/error stroke colors to `Border Error` when creating manual variants.
- Bind error text to `Text Error` when adding helper/error copy.

### Input tool call

```js
const page = await figma.getNodeByIdAsync('117:85');
await figma.setCurrentPageAsync(page);

const input = await figma.getNodeByIdAsync('130:4729'); // Email - Empty
const instance = input.createInstance();
instance.name = 'Input / Email';
instance.resize(364.4, 45.6);

return { createdNodeIds: [instance.id] };
```

Use `130:4733` when an invalid/error state is needed.

## Form sample

### Reference

- Sample node: `173:6491`
- Sample name: `Container`
- Source page: `0:1` (`v1.0.0`)
- Size: `555 × 806`
- Layout: vertical auto-layout
- Root sizing: **Hug contents** horizontally and vertically
- Use case: Create New Business / business registration form

### Structure

| Layer | Node ID | Size | Layout | Notes |
|---|---:|---:|---|---|
| Root `Container` | `173:6491` | `555 × 806` | Vertical, hug/hug | White form shell |
| `Section` | `173:6492` | `555 × 806` | Vertical, hug/hug | Padding `44 36 44 36` |
| `Form` | `173:6493` | `483 × 718` | Vertical, fill/hug | Form content stack, `16px` gap |
| Header `Container` | `173:6494` | `483 × 156` | Vertical, fill/hug | Icon, title, subtitle |
| Company type row | `173:6502` | `483 × 50` | Horizontal, fill/hug | Two selectable company options |
| Submit wrapper | `173:6532` | `483 × 52` | Vertical, fill/hug | Button container |
| Submit button | `173:6533` | `481 × 44` | Horizontal, fill/hug | `Button / Default - Icon Right`, label `DONE` |

### Fields included

| Field | Label node | Input node | Example value |
|---|---:|---:|---|
| Business name | `173:6516` | `173:6517` | `Your legal business name or company name` |
| TAX-ID | `173:6521` | `173:6522` | `012345678` |
| Address | `173:6790` | `173:6791` | `41 Phat Xich Long...` |
| Position | `173:6796` | `173:6797` | `Sale` |
| Contact | `173:6802` | `173:6803` | `Email / Phone` |

### Typography and tokens

- Title: `Archivo Bold`, `30px`
- Description: `Archivo Regular`, `14px`
- Field labels: `Archivo ExtraBold`, `12px`
- Field values: `Archivo Regular`, `14px`
- Submit button label: `Archivo SemiBold`, `14px`
- Selected company option background: `Primary Surface` (`VariableID:178:5800`)
- Selected company option/radio border: `Border Primary` (`VariableID:178:5796`)
- Submit button fill: `Primary` (`VariableID:131:4992`)

### Form sample usage rules

- Use this sample as the default reference for business-registration and company-information forms.
- Clone the whole root `Container` when a flow needs the same form shell, spacing, company type selector, field stack, and submit area.
- Keep the root container and form sections on **Hug contents** or **Fill container** sizing; avoid fixed sizing except for atomic controls such as radio circles and fixed-height inputs/buttons.
- Use **Fill container** for field rows and full-width buttons inside the form.
- Use **Hug contents** for labels, helper text, and internal content stacks.
- Replace manual field frames with the built `Input` component when the target field matches supported Input variants.
- For custom manual fields, keep the baseline field height at `45.6px`, padding `12 16`, fill `#F9FAFB`, and border `#E5E7EB` unless a state-specific token applies.
- Keep primary selected states bound to `Primary Surface`, `Border Primary`, and `Primary`.
- Convert the two `Inter Semi Bold` company-option labels to Archivo when cloning this sample for production-ready screens.

### Clone Form sample

```js
const page = await figma.getNodeByIdAsync('0:1');
await figma.setCurrentPageAsync(page);

const sample = await figma.getNodeByIdAsync('173:6491');
const form = sample.clone();
form.name = 'Form / Create New Business';
form.x = 0;
form.y = 0;

return { createdNodeIds: [form.id] };
```

### Form validation snippet

```js
const page = await figma.getNodeByIdAsync('0:1');
await figma.setCurrentPageAsync(page);

const form = await figma.getNodeByIdAsync('FORM_NODE_ID');
const textNodes = form.findAllWithCriteria({ types: ['TEXT'] });
const nonArchivo = [];

for (const text of textNodes) {
  for (const segment of text.getStyledTextSegments(['fontName'])) {
    if (segment.fontName.family !== 'Archivo') {
      nonArchivo.push({ id: text.id, name: text.name, font: segment.fontName });
    }
  }
}

return {
  nonArchivoCount: nonArchivo.length,
  nonArchivo,
};
```

For final screens, the expected result is `nonArchivoCount: 0`.

## Product Card

### Reference

- Component node: `128:4435`
- Component key: `0fed7ceca94b7e00884ca91811442f69f9b67b06`
- Size: `313.325 × 463.988`
- Layout mode: vertical auto-layout

### Recommended use

Use Product Card in product listing, category, search, and quote-building surfaces. Prefer creating instances from the component instead of manually rebuilding product card visuals.

When placing Product Cards in grids:

- Use **Fill container** for grid columns when the grid controls width.
- Use **Hug contents** for card internals if editing a local copy.
- Keep card instances visually aligned to the current `313.325 × 463.988` baseline unless the page layout requires responsive width.

### Product Card tool call

```js
const page = await figma.getNodeByIdAsync('117:85');
await figma.setCurrentPageAsync(page);

const productCard = await figma.getNodeByIdAsync('128:4435');
const instance = productCard.createInstance();
instance.name = 'Product Card';
instance.resize(313.325, 463.988);

return { createdNodeIds: [instance.id] };
```

## Header

### Reference

- Component set node: `117:169`
- Component set key: `8c58464569a06f65c93185486c2f021244123ec5`
- Size: `1536 × 105`

### Variants

| Variant | Node ID | Component key | Use case |
|---|---:|---|---|
| `Property 1=Guest` | `117:168` | `40d22dbc3e4e2ec1000e3d91e3c1d7bc7c3deadf` | Public pages, login/register flows, logged-out users |
| `Property 1=Authenticated` | `117:170` | `0daa15c04e1fb4b8f43d0f7a7d1267b0acdf11b5` | Account, company, quote, order, and authenticated flows |

### Header tool call

```js
const page = await figma.getNodeByIdAsync('117:85');
await figma.setCurrentPageAsync(page);

const header = await figma.getNodeByIdAsync('117:168'); // Guest
const instance = header.createInstance();
instance.name = 'Header / Guest';
instance.resize(1536, 105);

return { createdNodeIds: [instance.id] };
```

Use `117:170` for authenticated screens and name the instance `Header / Authenticated`.

## Certifications

### Reference

- Component node: `117:294`
- Component key: `3ef073c9a70e11be6677901636643d4de423fa4c`
- Size: `1536 × 204.8`

### Recommended use

Use Certifications near the bottom of MDFoods public and commerce pages, immediately before the Footer when the screen follows the standard MDFoods page structure.

### Certifications tool call

```js
const page = await figma.getNodeByIdAsync('117:85');
await figma.setCurrentPageAsync(page);

const certifications = await figma.getNodeByIdAsync('117:294');
const instance = certifications.createInstance();
instance.name = 'Certifications';
instance.resize(1536, 204.8);

return { createdNodeIds: [instance.id] };
```

## Footer

### Reference

- Component node: `117:403`
- Component key: `5401e0067839f1896765615ae6b0b2bed7379e29`
- Size: `1536 × 571.95`

### Recommended use

Use Footer as the final section of standard MDFoods pages. Place it after Certifications when Certifications is present.

### Footer tool call

```js
const page = await figma.getNodeByIdAsync('117:85');
await figma.setCurrentPageAsync(page);

const footer = await figma.getNodeByIdAsync('117:403');
const instance = footer.createInstance();
instance.name = 'Footer';
instance.resize(1536, 571.95);

return { createdNodeIds: [instance.id] };
```

## Page composition order

For a standard MDFoods page or flow screen:

1. `Header / Guest` or `Header / Authenticated`
2. Page-specific main content
3. `Certifications`
4. `Footer`

Use fixed width `1536` for desktop MDFoods web screens unless the target flow explicitly requires another width.
