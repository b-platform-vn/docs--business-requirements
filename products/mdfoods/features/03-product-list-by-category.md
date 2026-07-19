# Product List by Category

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Design** | [Figma](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9/MDFoods.vn?node-id=67-2&t=IlEz7GtYVk6JN31L-4) |

---

## The Problem

Business customers need to browse the food supplement catalog organized by category so they can quickly find the products relevant to their needs before requesting a quote.

- Customers shop by product type/category, not by scrolling an unstructured list.
- Bulk buyers need to filter and compare products within a category efficiently.

## Proposed Solution

A **Product List by Category** page that displays products grouped and filterable by category, sourced from the PMS via the API.

### Goals

- Enable efficient product discovery by category.
- Support filtering and sorting for bulk-buying decisions.

### Out-of-scope

- Product search across categories (see Product Search feature).
- Personalized product ranking.

### Measurable Outcomes

- Category page load time within target.
- Products-viewed-per-session measured.

## Requirements

### Category Browsing

- [P0] Display products grouped by category.
- [P0] Show product image, name, and starting price.
- [P0] Data sourced from PMS → MSSQL → API.

### Filtering & Sorting

- [P1] Filter by attributes (price range, product type).
- [P1] Sort by price, popularity, newest.
- [P2] Save filter preferences.

### Pagination

- [P0] Paginate long category lists.
- [P1] Infinite scroll option.

### Actions

- [P0] Click product to view Product Detail.
- [P1] Quick "Add to cart" from list.

## Appendix

_None yet._
