# Landing Page (Home Page)

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.08.05 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Figma** | [MDFoods.vn — Landing Page](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9/MDFoods.vn?node-id=60-2&t=K7EdQQCYiiIV9cyM-4) |

---

## Problem

Business customers arriving at MDFoods need a clear entry point that communicates what the platform offers, highlights current promotions, and guides them toward product discovery. Without a well-structured home page:

- New visitors don't immediately understand that MDFoods is a B2B wholesale platform for food supplement products.
- Returning customers want quick access to promotions, new arrivals, and popular items.
- Product discovery suffers without clear category navigation on the landing page.

## Target Audiences

- **B2B buyers / business customers** — need to quickly find product categories and promotions relevant to their purchasing needs.
- **New visitors** — need to understand MDFoods' offering and start browsing or registering.
- **Returning customers** — need fast access to new products, featured items, and active deals.

## Solution

A content-rich **Landing Page** below the Master Layout, organized in 6 sections (top to bottom):

1. **Level 1 Categories** — Visual grid of top-level product categories.
2. **Hot Deal Banner** — Full-width promotional banner linking to a specific promotion.
3. **Hot Promotions** — Product cards currently on promotion.
4. **Featured Products** — Curated product carousel.
5. **New Products** — Recently added product carousel.
6. **Popular Products** — Best-selling product carousel.

## Goals (Business Impact)

- Drive product discovery through visible category navigation on the first screen.
- Increase promotion engagement by surfacing active deals prominently.
- Encourage quote requests by showing "Add to quote" on every product card.
- Reduce time-to-first-action for returning buyers via curated product sections.
- Communicate the B2B wholesale nature of the platform (price shown as "Contact for Price").

## Functions/Usecases

### 1. Level 1 Category Grid

- **Load categories from PMS** — When the user lands on the home page, the system loads all **Level 1 product categories** from the Product Management System (PMS).
- **Auto-slider display** — User sees the Level 1 categories rendered as an auto-sliding category carousel/grid. Each category item shows a representative image and category name.
- **Auto-slide behavior** — The category slider automatically advances through category items without requiring user interaction.
- **Manual navigation** — User can manually move through the category slider using swipe/scroll or navigation controls when available.
- **Category click** — When user clicks a category item, they are taken to that category's product list page (`/{locale}/category/{category-slug}`).

### 2. Hot Deal Banner

- **Banner display** — User sees a full-width promotional banner image (e.g. "Hotdeal on website launching").
- **Banner click** — When user clicks the banner, they are taken to the specific promotion detail page (`/{locale}/promotions/{id}`).

### 3. Hot Promotions

- **Section title** — User sees "Hot Promotions" heading with an "All promotions" link.
- **Promotion product cards** — User sees product cards for items currently on promotion. Each card shows: product image, SKU, product name, price (or "Contact for Price"), and an "Add to quote" button.
- **"All promotions" link** — When user clicks it, they are taken to the promotions listing page (`/{locale}/promotions`).
- **Product card click** — When user clicks a product card image or name, they are taken to the product detail page (`/{locale}/products/{slug}`).
- **"Add to quote" button** — When user clicks it, the product is added to their quote cart.

### 4. Featured Products

- **Section title** — User sees "Featured products" heading with an "All products" link.
- **Product carousel** — User sees a horizontally-scrollable carousel of featured product cards. Each card shows: product image, SKU, product name, price (or "Contact for Price"), and an "Add to quote" button.
- **"All products" link** — When user clicks it, they are taken to the product list filtered by featured (`/{locale}/category?productType=featured`).
- **Product card click** — When user clicks a product card image or name, they are taken to the product detail page.
- **"Add to quote" button** — When user clicks it, the product is added to their quote cart.

### 5. New Products

- **Section title** — User sees "New products" heading with an "All products" link.
- **Product carousel** — User sees a horizontally-scrollable carousel of newly-added product cards. Each card shows: product image, SKU, product name, price (or "Contact for Price"), and an "Add to quote" button.
- **"All products" link** — When user clicks it, they are taken to the product list filtered by new (`/{locale}/category?productType=new`).
- **Product card click** — When user clicks a product card image or name, they are taken to the product detail page.
- **"Add to quote" button** — When user clicks it, the product is added to their quote cart.

### 6. Popular Products

- **Section title** — User sees "Popular products" heading with an "All products" link.
- **Product carousel** — User sees a horizontally-scrollable carousel of best-selling product cards. Each card shows: product image, SKU, product name, price (or "Contact for Price"), and an "Add to quote" button.
- **"All products" link** — When user clicks it, they are taken to the product list filtered by popular (`/{locale}/category?productType=popular`).
- **Product card click** — When user clicks a product card image or name, they are taken to the product detail page.
- **"Add to quote" button** — When user clicks it, the product is added to their quote cart.

### Product Card (shared component)

Each product card across sections 3–6 displays:

- Product image (clickable → product detail page)
- SKU code (format: `SKU: {code}`)
- Product name (clickable → product detail page)
- Price — either a numeric price or "Contact for Price" for B2B items
- "Add to quote" button

### Carousel Navigation

- **Scroll/swipe** — User can scroll product carousels horizontally (mouse drag, touch swipe, or trackpad).
- **Arrow controls** — User sees left/right navigation arrows on desktop to advance the carousel.
