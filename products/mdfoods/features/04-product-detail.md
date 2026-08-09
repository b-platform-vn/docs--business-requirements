# Product Detail

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.08.05 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Design** | [Figma](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9/MDFoods.vn?node-id=70-2) |

---

## The Problem

Before adding a product to the quote cart, business customers need complete information about it — specifications, pricing tiers, and availability — to make informed bulk-purchasing decisions.

- Bulk buyers need detailed specs and volume pricing before committing.
- Customers want to see all product attributes managed in the PMS.

## Proposed Solution

A **Product Detail** page presenting full product information sourced from the PMS, with a clear action to add the product to the quote cart.

### Goals

- Provide all information needed for a bulk-purchase decision.
- Drive conversion to cart / quote request.

### Out-of-scope

- User-generated reviews (future).
- Live inventory sync (depends on PMS capability).

### Measurable Outcomes

- Add-to-quote conversion rate from the detail page.
- Time-on-page for detail views.

## Requirements

### Product Information

- [P0] Display name, SKU, description, images.
- [P0] Show pricing tiers / volume pricing indication.
- [P0] Data sourced from PMS → MSSQL → API.
- [P1] Specifications table (ingredients, packaging, etc.).
- [P2] Related / recommended products.

### Actions

- [P0] Add to quote with quantity selector.
- [P0] Request quote CTA from the quote cart.
- [P1] Share / save product.

## Appendix

_None yet._
