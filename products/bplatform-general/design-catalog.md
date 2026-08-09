# B-Platform Design Catalog (Sneat Admin Reference)

This catalog documents the component-level design system captured from the Sneat Vuetify NuxtJS Admin Template (`demos.themeselection.com/sneat-vuetify-nuxtjs-admin-template/demo-1`), used as the exact-reproduction visual reference for B-Platform Super App Figma designs.

- Reference product: Sneat NuxtJS Admin Template, Demo 1
- Captured on: 2026-08-06
- Coverage: all ~112 routes reachable from the left navigation were crawled and confirmed to load; representative pages per component category were deep-inspected for computed styles
- Companion file: `products/bplatform-general/figma_guide.md` (Figma-specific workflow and file references)

## How this catalog was produced

- Full left-navigation route map was extracted by expanding every nav group and collecting `href` values.
- Every route was visited and checked for successful load (no 404s across all 112 pages).
- Canonical/representative pages per category (Typography, Buttons, Inputs, Tables, Cards, Dashboards, Auth) were inspected with computed-style extraction (`getComputedStyle`) across desktop, iPad, and mobile viewport widths.
- Values below are computed pixel/RGB values as rendered by the browser, not source Sass variables — use them as an exact-reproduction reference.

---

## 1. Page inventory (full crawl)

### Dashboards
| Page | Route |
|---|---|
| Analytics | `/dashboards/analytics` |
| CRM | `/dashboards/crm` |
| Ecommerce | `/dashboards/ecommerce` |
| Academy | `/dashboards/academy` |
| Logistics | `/dashboards/logistics` |

### Front Pages
| Page | Route |
|---|---|
| Landing Page | `/front-pages/landing-page` |
| Pricing | `/front-pages/pricing` |
| Payment | `/front-pages/payment` |
| Checkout | `/front-pages/checkout` |
| Help Center | `/front-pages/help-center` |

### Apps & Pages
| Page | Route |
|---|---|
| Ecommerce Dashboard | `/apps/ecommerce/dashboard` |
| Product List | `/apps/ecommerce/product/list` |
| Product Add | `/apps/ecommerce/product/add` |
| Product Category List | `/apps/ecommerce/product/category-list` |
| Order List | `/apps/ecommerce/order/list` |
| Order Details | `/apps/ecommerce/order/details/:id` |
| Customer List | `/apps/ecommerce/customer/list` |
| Customer Details | `/apps/ecommerce/customer/details/:id` |
| Manage Review | `/apps/ecommerce/manage-review` |
| Referrals | `/apps/ecommerce/referrals` |
| Ecommerce Settings | `/apps/ecommerce/settings` |
| Academy Dashboard | `/apps/academy/dashboard` |
| My Course | `/apps/academy/my-course` |
| Course Details | `/apps/academy/course-details` |
| Logistics Dashboard | `/apps/logistics/dashboard` |
| Fleet | `/apps/logistics/fleet` |
| Email | `/apps/email` |
| Chat | `/apps/chat` |
| Calendar | `/apps/calendar` |
| Kanban | `/apps/kanban` |
| Invoice List | `/apps/invoice/list` |
| Invoice Preview | `/apps/invoice/preview/:id` |
| Invoice Edit | `/apps/invoice/edit/:id` |
| Invoice Add | `/apps/invoice/add` |
| User List | `/apps/user/list` |
| User View | `/apps/user/view/:id` |
| Roles | `/apps/roles` |
| Permissions | `/apps/permissions` |

### Pages
| Page | Route |
|---|---|
| User Profile | `/pages/user-profile/profile` |
| Account Settings | `/pages/account-settings/account` |
| Pricing | `/pages/pricing` |
| FAQ | `/pages/faq` |
| Coming Soon | `/pages/misc/coming-soon` |
| Under Maintenance | `/pages/misc/under-maintenance` |
| Not Authorized (401) | `/pages/misc/not-authorized` |
| Dialog Examples | `/pages/dialog-examples` |
| Typography | `/pages/typography` |
| Icons | `/pages/icons` |
| Card Basic | `/pages/cards/card-basic` |
| Card Advance | `/pages/cards/card-advance` |
| Card Statistics | `/pages/cards/card-statistics` |
| Card Widgets | `/pages/cards/card-widgets` |
| Card Gamifications | `/pages/cards/card-gamifications` |
| Card Actions | `/pages/cards/card-actions` |

### Authentication
| Page | Route |
|---|---|
| Login v1 / v2 | `/pages/authentication/login-v1`, `login-v2` |
| Register v1 / v2 | `/pages/authentication/register-v1`, `register-v2` |
| Register Multi-Steps | `/pages/authentication/register-multi-steps` |
| Verify Email v1 / v2 | `/pages/authentication/verify-email-v1`, `verify-email-v2` |
| Forgot Password v1 / v2 | `/pages/authentication/forgot-password-v1`, `forgot-password-v2` |
| Reset Password v1 / v2 | `/pages/authentication/reset-password-v1`, `reset-password-v2` |
| Two Steps v1 / v2 | `/pages/authentication/two-steps-v1`, `two-steps-v2` |

### Wizard Examples
| Page | Route |
|---|---|
| Checkout Wizard | `/wizard-examples/checkout` |
| Property Listing Wizard | `/wizard-examples/property-listing` |
| Create Deal Wizard | `/wizard-examples/create-deal` |

### UI Elements — Components
| Page | Route |
|---|---|
| Alert | `/components/alert` |
| Avatar | `/components/avatar` |
| Badge | `/components/badge` |
| Button | `/components/button` |
| Chip | `/components/chip` |
| Dialog | `/components/dialog` |
| Expansion Panel | `/components/expansion-panel` |
| List | `/components/list` |
| Menu | `/components/menu` |
| Pagination | `/components/pagination` |
| Progress Circular | `/components/progress-circular` |
| Progress Linear | `/components/progress-linear` |
| Snackbar | `/components/snackbar` |
| Tabs | `/components/tabs` |
| Timeline | `/components/timeline` |
| Tooltip | `/components/tooltip` |

### Extensions
| Page | Route |
|---|---|
| Tour | `/extensions/tour` |
| Swiper | `/extensions/swiper` |

### Forms & Tables
| Page | Route |
|---|---|
| Autocomplete | `/forms/autocomplete` |
| Checkbox | `/forms/checkbox` |
| Combobox | `/forms/combobox` |
| Date/Time Picker | `/forms/date-time-picker` |
| Editors | `/forms/editors` |
| File Input | `/forms/file-input` |
| Radio | `/forms/radio` |
| Custom Input | `/forms/custom-input` |
| Range Slider | `/forms/range-slider` |
| Rating | `/forms/rating` |
| Select | `/forms/select` |
| Slider | `/forms/slider` |
| Switch | `/forms/switch` |
| Textarea | `/forms/textarea` |
| Text Field | `/forms/textfield` |
| Form Layouts | `/forms/form-layouts` |
| Form Wizard (Numbered) | `/forms/form-wizard-numbered` |
| Form Wizard (Icons) | `/forms/form-wizard-icons` |
| Form Validation | `/forms/form-validation` |
| Simple Table | `/tables/simple-table` |
| Data Table | `/tables/data-table` |

### Charts & Others
| Page | Route |
|---|---|
| ApexCharts | `/charts/apex-chart` |
| Chart.js | `/charts/chartjs` |
| Access Control | `/access-control` |

All 112 routes above were verified reachable with no 404 responses during this crawl.

---

## 2. Colors

Core semantic theme tokens (RGB, light theme):

| Token | RGB | Hex | Usage |
|---|---|---|---|
| Primary | `105, 108, 255` | `#696CFF` | Primary actions, active nav state, links |
| On Primary | `255, 255, 255` | `#FFFFFF` | Text/icons on primary fill |
| Secondary | `133, 146, 163` | `#8592A3` | Secondary actions, muted UI |
| Success | `113, 221, 55` | `#71DD37` | Positive states, success badges |
| Info | `3, 195, 236` | `#03C3EC` | Informational states |
| Warning | `255, 171, 0` | `#FFAB00` | Warning states |
| Error | `255, 62, 29` | `#FF3E1D` | Error/destructive states |
| Background | `245, 245, 249` | `#F5F5F9` | App shell background |
| Surface | `255, 255, 255` | `#FFFFFF` | Card/panel/table surface |
| On Surface | `34, 48, 62` | `#22303E` | Primary text on white surfaces |
| On Background | `34, 48, 62` | `#22303E` | Text on shell background |

### Text opacity scale (on `#22303E` base)

Vuetify applies opacity rather than distinct hex values for text hierarchy:

| Role | Value | Usage |
|---|---|---|
| High emphasis | `rgba(34, 48, 62, 0.9)` | Headings, primary labels, button text |
| Medium emphasis | `rgba(34, 48, 62, 0.7)` | Body text, secondary text |
| Disabled | `rgba(34, 48, 62, 0.4)` | Disabled text/icons |
| Border / divider | `rgba(34, 48, 62, 0.12)` | Card borders, table row dividers |

### Neutral / grey scale

| Token | RGB | Hex |
|---|---|---|
| Grey 50 | `250, 250, 250` | `#FAFAFA` |
| Grey 100 | `245, 245, 245` | `#F5F5F5` |
| Grey 200 | `238, 238, 238` | `#EEEEEE` |
| Grey 300 | `224, 224, 224` | `#E0E0E0` |
| Grey 400 | `189, 189, 189` | `#BDBDBD` |
| Grey 500 | `158, 158, 158` | `#9E9E9E` |
| Grey 600 | `117, 117, 117` | `#757575` |
| Grey 700 | `97, 97, 97` | `#616161` |
| Grey 800 | `66, 66, 66` | `#424242` |
| Grey 900 | `33, 33, 33` | `#212121` |

### Color usage rules

- Use `Primary` for filled primary buttons, active nav items, links, focus rings.
- Use semantic colors (`success`, `info`, `warning`, `error`) consistently for status chips, alerts, and badges — never invent new status colors.
- Use the opacity-based text scale instead of separate greys for on-surface text, to match the reference exactly.
- Card and table borders use `rgba(34, 48, 62, 0.12)`, not a solid grey — reproduce this as a semi-transparent stroke in Figma, or bind to an equivalent low-opacity variable.

---

## 3. Typography

Font family: **`Public Sans`** (fallback stack: `sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial`).

### Heading scale (from `/pages/typography`)

| Style | Font size | Font weight | Line height | Letter spacing |
|---|---:|---:|---:|---|
| H1 | `46px` | `500` | `68px` | normal |
| H2 | `38px` | `500` | `56px` | normal |
| H3 | `28px` | `500` | `42px` | normal |
| H4 | `24px` | `500` | `38px` | normal |
| H5 | `18px` | `500` | `28px` | normal |
| H6 | `15px` | `500` | `22px` | normal |

### Body / utility text styles

| Style | Font size | Font weight | Notes |
|---|---:|---:|---|
| Body text (default `p`) | `15px` | `400` | Standard paragraph, `0.7` text opacity |
| Table header / label | `13px` | `500` | Uppercase transform, `0.9` text opacity |
| Table body cell | `13px`–`15px` | `400`–`500` | Depends on table density |
| Field label | `13px` | `400` | Input floating label |
| Button label | `15px` | `500` | No text-transform (sentence case as authored) |

### Typography usage rules

- Use `Public Sans` for all B-Platform Super App UI text (unless a product overrides it, e.g. MDFoods uses `Archivo`).
- Preserve the heading scale exactly: `46/38/28/24/18/15` px for H1–H6.
- Use `500` weight for headings, `400` for body text, `500` for button and table-header labels.
- Table headers use `13px`, uppercase, `500` weight, high-emphasis text color.

---

## 4. Buttons

Reference page: `/components/button`. Captured **40+ live button instances** across variants.

### Variant matrix

| Variant | Background | Text color | Border radius | Shadow |
|---|---|---|---:|---|
| **Elevated / Flat** (filled) | Solid semantic color (e.g. `rgb(105,108,255)` for primary) | White | `6px` | Has shadow (elevated only; flat has none) |
| **Outlined** | Transparent | Semantic color | `6px` | None |
| **Text** | Transparent | Semantic color or `rgba(34,48,62,0.9)` for default | `4px` (or `50%` for icon buttons) | None |
| **Icon button** (text variant, round) | Transparent | Inherits | `50%` | None |
| **Disabled (text)** | Transparent | `rgba(34,48,62,0.4)` | `50%` | None |

### Semantic color fills (elevated/flat buttons)

| Semantic | Background |
|---|---|
| Primary | `rgb(105, 108, 255)` |
| Secondary | `rgb(133, 146, 163)` |
| Success | `rgb(113, 221, 55)` |
| Info | `rgb(3, 195, 236)` |
| Warning | `rgb(255, 171, 0)` |
| Error | `rgb(255, 62, 29)` |

### Button rules

- Default corner radius for standard (elevated/flat/outlined) buttons: **`6px`**.
- Icon-only / round buttons use **`50%`** radius (fully circular).
- Text-transform: **none** — button labels are shown in their authored case (not forced uppercase), font weight `500`, `15px`.
- Use elevated (with shadow) for primary emphasis CTAs; use flat/outlined/text for secondary and tertiary actions.
- Reuse the six semantic colors (primary/secondary/success/info/warning/error) — do not introduce new button colors outside this set.

---

## 5. Inputs (Text Fields)

Reference page: `/forms/textfield`. Vuetify field variants captured:

| Variant | Background | Border | Border radius | Height | Shadow |
|---|---|---|---:|---:|---|
| **Outlined** (default) | Transparent | None visible in rest state (outline drawn via pseudo-border) | `6px` | `38px` | None |
| **Filled** | Transparent (filled tone applied via layered background) | None | `6px 6px 0 0` (top corners only) | `39px` | None |
| **Solo** | White `rgb(255,255,255)` | None | `6px` | `39px` | Has shadow (elevated card-like input) |
| **Plain** | Transparent | None | `0px` | `31px` | None |
| **Underlined** | Transparent | Bottom border only | `0px` | `31px` | None |

### Label

- Field label: `13px`, `400` weight, high-emphasis text color `rgba(34,48,62,0.9)`.

### Input usage rules

- Default to the **Outlined** variant for standard forms (most common across `/forms/form-layouts` and admin forms).
- Use **Solo** variant only for elevated/standalone inputs (e.g., global search bar) since it carries its own shadow and white background.
- Keep consistent **`6px`** corner radius across outlined/filled/solo variants to match buttons and cards.
- Use the same `Public Sans` font family and `13–15px` sizing as body text.

---

## 6. Tables

Two reference pages: `/tables/simple-table` and `/tables/data-table`.

| Element | Style |
|---|---|
| Header cell | `13px`, weight `500`, **uppercase**, text `rgba(34,48,62,0.9)`, padding `0px 16px 0px 20px`, bottom border `0.8px solid rgba(34,48,62,0.12)` |
| Body cell (simple table) | `15px`, weight `400`, text `rgba(34,48,62,0.7)`, same padding, same `0.8px` border |
| Body cell (data table) | `13px`, weight `500` (denser data-grid style) |
| Table wrapper / surface | White background, `15px` base font, no outer border by default (relies on containing card) |

### Table usage rules

- Always place tables inside a card/container with `6px` radius and the standard `rgba(34,48,62,0.12)` border.
- Header row: uppercase labels, `13px`, `500` weight — do not use sentence case for table headers.
- Row dividers use a **hairline** `0.8px` border in the reference border color, not a full `1px` grey rule.
- Use the denser **Data Table** cell style (`13px`/`500`) for admin data grids with actions/checkboxes; use the **Simple Table** style (`15px`/`400`) for lightweight/report-style tables.

---

## 7. Cards / Containers

Reference: CRM dashboard card sample plus `/pages/cards/*` gallery (card-basic, card-advance, card-statistics, card-widgets, card-gamifications, card-actions).

| Property | Value |
|---|---|
| Background | `rgb(255, 255, 255)` |
| Border | `0px solid rgba(34, 48, 62, 0.12)` (hairline, often visually implied via shadow instead of a hard border) |
| Border radius | `6px` |
| Text color | `rgba(34, 48, 62, 0.7)` body / `rgba(34,48,62,0.9)` headings |
| Font size | `15px` base |

### Layout shell containers

| Region | Value |
|---|---|
| Sidebar width | `260px` |
| Topbar horizontal padding | `24px` |
| Page content padding | `24px` |
| Page content top margin (below topbar) | `16px` |
| Desktop content width (1536 viewport) | `1152px` effective content column |

### Container / card usage rules

- Use `6px` corner radius consistently for cards, buttons, and inputs — this is the template's single global radius token.
- Keep card backgrounds pure white against the `#F5F5F9` page background for clear layering.
- Use `24px` page padding and `16px` gap between the topbar and first content row as the standard shell rhythm (already reflected in `products/bplatform-general/figma_guide.md`).

---

## 8. Layout principles

- **Shell structure**: fixed left sidebar (`260px`) + top app bar + fluid main content area. This holds across all dashboard, apps, and admin pages.
- **Responsive behavior**: verified at desktop (1536w), iPad (≈1024w), and mobile widths — the sidebar width (`260px`) stays fixed on tablet, while content column width scales down proportionally (e.g., card width scaled from `540px` desktop to `373.6px` on iPad).
- **Spacing rhythm**: `24px` outer page padding, `16px` inter-section gaps, `6px` corner radius as the single shared radius token across cards/buttons/inputs.
- **Elevation**: only "solo" inputs and "elevated" buttons carry shadows; cards rely primarily on the hairline border + white-on-grey contrast rather than heavy shadows.
- **Content density**: admin data screens (tables, lists) use denser `13px` type; marketing/front-page content uses larger `15px`+ type.
- **Front Pages vs Admin Pages**: Front Pages (`landing-page`, `pricing`, `payment`, `checkout`, `help-center`) use a public marketing layout without the admin shell (no sidebar), while all `/dashboards`, `/apps`, and `/pages` routes use the standard admin shell.

---

## 9. Sample pages by pattern (for B-Platform reuse)

| B-Platform need | Closest Sneat reference page |
|---|---|
| Admin dashboard / KPI overview | `/dashboards/crm`, `/dashboards/analytics` |
| Data management list (e.g. Users, Roles) | `/apps/user/list`, `/apps/roles`, `/apps/permissions` |
| Record detail / profile view | `/apps/user/view/:id`, `/apps/ecommerce/customer/details/:id` |
| Settings / account management | `/pages/account-settings/account` |
| Sign-in / auth flows | `/pages/authentication/login-v1`, `login-v2` |
| Multi-step registration or onboarding | `/pages/authentication/register-multi-steps`, `/wizard-examples/*` |
| Simple listing table | `/tables/simple-table` |
| Dense data grid with filters/actions | `/tables/data-table`, `/apps/ecommerce/product/list` |
| Form-heavy page (create/edit) | `/forms/form-layouts`, `/apps/ecommerce/product/add` |
| Empty/error/status states | `/pages/misc/coming-soon`, `/pages/misc/not-authorized`, `/pages/misc/under-maintenance` |
| Notification/messaging | `/apps/email`, `/apps/chat` |
| Kanban / pipeline board | `/apps/kanban` |
| Calendar / scheduling | `/apps/calendar` |
| Invoice / billing document | `/apps/invoice/list`, `/apps/invoice/preview/:id` |

---

## 10. Open gaps / not yet deep-captured

The following were confirmed to load correctly but were not deep-style-inspected (only counted for cards/tables/buttons/fields as a fingerprint check). Revisit if a B-Platform screen needs to closely match one of these:

- Chat, Email, Calendar, Kanban app interaction patterns (drag-and-drop, threads, composer UI)
- Chart styling (`ApexCharts`, `Chart.js` visual specifics — colors/gridlines/tooltips)
- Icon set specifics on `/pages/icons`
- Wizard step-indicator visual states (`/wizard-examples/*`, `/forms/form-wizard-*`)
- Dialog/snackbar/tooltip/menu exact positioning and animation behavior

If any of these are needed for a specific B-Platform feature, capture that page specifically before designing the Figma equivalent.
