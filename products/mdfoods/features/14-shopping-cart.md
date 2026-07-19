# Shopping Cart

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |

---

## The Problem

Business customers assemble bulk orders across multiple products. They need a cart to collect items, adjust quantities, and review before requesting a quote or placing an order.

- Bulk orders span multiple products and large quantities.
- Customers need to review and adjust before committing to a quote/order.
- Cart must persist across sessions for logged-in users.

## Proposed Solution

A **Shopping Cart** where customers add products, adjust quantities, and proceed to quote request or order.

### Goals

- Enable multi-product order assembly.
- Persist cart for logged-in users.
- Smooth transition to quote/order.

### Out-of-scope

- Wishlist / saved-for-later (future).
- Multiple concurrent carts.

### Measurable Outcomes

- Cart-to-quote / cart-to-order conversion rate.
- Cart abandonment rate.

## Requirements

### Cart Operations

- [P0] Add product with quantity.
- [P0] Update item quantity.
- [P0] Remove item.
- [P0] Display cart subtotal / estimated total.
- [P1] Clear entire cart.

### Persistence

- [P0] Server-side cart for logged-in users.
- [P1] Local cart for guests (merged on login).

### Actions

- [P0] Proceed to Request Quote.
- [P0] Proceed to Order (if pricing available).

## Appendix

_None yet._
