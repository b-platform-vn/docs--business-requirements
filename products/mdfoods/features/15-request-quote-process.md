# Request Quote Process

| | |
|---|---|
| **Author** | _TBD_ |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |

---

## The Problem

The core value proposition of MDFoods is volume-based pricing — larger orders get better prices. Customers need a way to submit their cart for pricing evaluation so the business can provide a tailored quote based on quantity and customer type.

- Standard pricing doesn't reflect B2B volume discounts.
- Each customer/order may warrant different negotiated pricing.
- This is the primary conversion action on the platform.

## Proposed Solution

A **Request Quote Process** that lets customers submit their cart (products + quantities) for volume-based pricing. The system or a sales agent responds with a quoted price.

### Goals

- Enable the core B2B pricing model (volume-based / negotiated).
- Streamline quote requests from cart to submission.
- Provide clear quote status and timeline to customer.

### Out-of-scope

- Automated pricing engine logic (backend concern).
- Quote negotiation chat (handled via CRM messaging).

### Measurable Outcomes

- Quote request volume.
- Average time from request to quote response.
- Quote-to-order conversion rate.

## Requirements

### Quote Request Flow

- [P0] Customer proceeds from cart to "Request Quote."
- [P0] Display summary of items and quantities being quoted.
- [P0] Select or confirm delivery address.
- [P0] Submit quote request.
- [P0] Show confirmation with expected response timeline.

### Quote Information

- [P0] Capture: products, quantities, customer type, delivery address.
- [P1] Add notes / special instructions.
- [P1] Select preferred response channel (in-app, email).

### Status

- [P0] Quote request status visible in customer account (Pending, Quoted, Expired).
- [P1] Email/notification when quote response arrives.

## Appendix

### Quote Flow Diagram

```
Cart → Request Quote → Review Items → Confirm Address → Submit
→ Pending → Quote Received (notification) → Accept / Reject → Order
```
