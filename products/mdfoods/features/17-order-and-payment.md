# Order & Payment

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |

---

## The Problem

Once a customer accepts a quote, they need to convert it into a confirmed order and complete payment. The B2B context means payment methods may include bank transfer, invoicing, or online payment — not just instant credit card processing.

- Quote acceptance must seamlessly transition to order placement.
- B2B payment methods differ from B2C (bank transfer, net-30, etc.).
- Order confirmation must be clear with all terms visible.

## Proposed Solution

An **Order & Payment** flow that converts an accepted quote into a confirmed order with support for B2B-appropriate payment methods.

### Goals

- Seamless quote-to-order conversion.
- Support multiple B2B payment methods.
- Clear order confirmation and receipt.

### Out-of-scope

- Payment gateway integration details (backend concern).
- Credit / net-30 terms engine (future).

### Measurable Outcomes

- Order completion rate (from accepted quote).
- Payment success rate.
- Average time from quote acceptance to order confirmation.

## Requirements

### Order Placement

- [P0] Convert accepted quote into order.
- [P0] Display order summary (items, quantities, quoted prices, delivery address).
- [P0] Confirm order.
- [P0] Generate order reference number.

### Payment

- [P0] Select payment method: Bank transfer, Online payment.
- [P0] Show payment instructions (for bank transfer).
- [P1] Online payment gateway integration.
- [P1] Payment status tracking.

### Confirmation

- [P0] Order confirmation page with order details.
- [P0] Confirmation email sent.
- [P1] Downloadable invoice / order summary PDF.

## Appendix

_None yet._
