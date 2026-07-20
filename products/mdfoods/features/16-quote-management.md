# Quote Management

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Tracklogs** | [Multica BPL-253](https://tasks.tvlong.me/issues/BPL-253) |

---

## The Problem

After requesting a quote, customers need to track, compare, and act on received quotes. Without a management interface, they rely on email and lose visibility into quote history and expiry.

- Customers request multiple quotes over time.
- Quotes have expiry dates — customers must act before they expire.
- Comparing quotes or requesting revisions should be self-service.

## Proposed Solution

A **Quote Management** dashboard where customers view all quotes, their status, and can accept, reject, or request revision.

### Goals

- Full visibility into quote lifecycle.
- Self-service acceptance and rejection.
- Prevent missed expirations.

### Out-of-scope

- Quote negotiation chat (handled via CRM messaging, Feature 19).
- Batch quote operations.

### Measurable Outcomes

- Quote acceptance rate.
- Average time from quote received to action.
- Expired quote percentage (target: reduce).

## Requirements

### Quote List

- [P0] View all quotes (Pending, Quoted, Accepted, Expired, Rejected).
- [P0] Filter/sort by status, date.
- [P0] Show quote summary: items, total, expiry date.

### Quote Actions

- [P0] Accept quote → proceed to Order & Payment.
- [P0] Reject quote.
- [P1] Request revision (edit quantities, add notes).
- [P1] View quote history (previous versions).

### Lifecycle

- [P0] Status transitions: Pending → Quoted → Accepted / Rejected / Expired.
- [P1] Notification on expiry approaching.

## Appendix

_None yet._
