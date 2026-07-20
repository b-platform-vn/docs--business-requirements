# Addresses Management

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |

| **Tracklogs** | [Multica BPL-248](https://tasks.tvlong.me/issues/BPL-248) |
---

## The Problem

Business customers often ship to multiple warehouses or office locations. They need an address book to save, edit, and select addresses quickly during checkout and quote requests.

- Bulk orders may ship to different locations.
- Re-entering addresses each time causes friction and errors.

## Proposed Solution

An **Addresses Management** section allowing users to maintain a personal address book with multiple shipping and billing addresses.

### Goals

- Reduce checkout friction by pre-saving addresses.
- Support multi-location businesses.
- Prevent address-entry errors.

### Out-of-scope

- Address validation via external API (future).
- Map-based address picker.

### Measurable Outcomes

- Addresses saved per user.
- Checkout time reduction for repeat orders.

## Requirements

### Address Book

- [P0] Add new address (name, street, city, state, postal code, country, phone).
- [P0] Edit existing addresses.
- [P0] Delete addresses.
- [P0] Set default shipping and billing address.
- [P1] Label addresses (e.g., "Warehouse A", "HQ").

### Checkout Integration

- [P0] Select from saved addresses during order / quote.
- [P1] Add new address inline during checkout.

## Appendix

_None yet._
