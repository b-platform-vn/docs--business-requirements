# Orders Management

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |

| **Tracklogs** | [Multica BPL-255](https://tasks.tvlong.me/issues/BPL-255) |
---

## The Problem

After placing orders, business customers need to track status, view history, retrieve invoices, and reorder. Without an orders dashboard, they cannot self-serve on post-purchase needs.

- Bulk buyers place recurring orders and need history.
- Order status visibility (processing, shipped, delivered) reduces support inquiries.
- Invoices are needed for accounting.

## Proposed Solution

An **Orders Management** dashboard where customers view order history, track status, download invoices, and reorder.

### Goals

- Full post-purchase visibility.
- Self-service invoice retrieval.
- Enable quick reordering.

### Out-of-scope

- Real-time carrier tracking integration (future).
- Returns processing UI (may link to Returns policy).

### Measurable Outcomes

- Reorder rate.
- Support tickets for order status (target: reduce).
- Invoice downloads.

## Requirements

### Order List

- [P0] View all orders with status.
- [P0] Filter/sort by status, date, amount.
- [P0] Show order summary: reference, date, total, status.

### Order Detail

- [P0] View full order details (items, quantities, prices, delivery address).
- [P0] Display order status timeline (Placed → Processing → Shipped → Delivered).
- [P1] Download invoice / receipt.

### Actions

- [P1] Reorder (add order items back to cart).
- [P2] Request return / exchange (link to policy).

## Appendix

_None yet._
