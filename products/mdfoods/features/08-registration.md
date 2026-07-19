# Registration

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Figma** | [Design](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9?node-id=89-2) |

---

## The Problem

To place bulk orders and receive quotes, business customers must have accounts. The platform serves two distinct customer types — Individual Business and Enterprise Business — each with different registration needs.

- Bulk ordering and quoting require an authenticated business account.
- Individual and Enterprise customers have different data requirements.
- Registration must create a corresponding customer record in the CRM.

## Proposed Solution

A **Registration** flow supporting both Individual Business and Enterprise Business account types, integrated with the CRM to create customer records.

### Goals

- Enable business customers to self-register.
- Capture the right data per customer type.
- Sync new customers to the CRM.

### Out-of-scope

- Social login / SSO (future).
- Guest checkout (B2B requires accounts).

### Measurable Outcomes

- Registration completion rate.
- Drop-off rate per registration step.

## Requirements

### Registration Flow

- [P0] Select customer type: Individual Business or Enterprise Business.
- [P0] Collect basic account details (name, email, password, phone).
- [P0] Enterprise: collect company information (name, tax ID).
- [P0] Create customer record in CRM on successful registration.
- [P0] Email verification.
- [P1] Terms and conditions acceptance.
- [P2] Progressive/multi-step form with save progress.

### Validation

- [P0] Email uniqueness and format validation.
- [P0] Password strength requirements.
- [P1] Tax ID / company validation for Enterprise.

## Appendix

### Customer Types

- **Individual Business** — Sole proprietors or small buyers; lighter data requirements.
- **Enterprise Business** — Larger organizations requiring company info and team member management.
