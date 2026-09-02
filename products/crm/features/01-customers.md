# Customers

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.09.02 |
| **Product** | [CRM](/products/crm/README.md) |
| **Priority** | P1 |

---

## The Problem

Sale Master needs one place to manage the customer master list and a single customer record across channels. Without a dedicated Customers capability, customer data becomes fragmented, hard to maintain, and difficult to use for sales follow-up.

This capability is the **destination** for sourced customers: records discovered by the Rocket Agent customer-pool workflow should land here for review, maintenance, and outreach.

## Proposed Solution

A **Customers** capability that allows Sale Master to view, edit, and remove customer records from one CRM workspace.

This capability owns the canonical customer master list used after sourcing/enrichment.

### Goals

- Show all available customers in one list.
- Expose key customer summary fields for quick sales work.
- Allow controlled editing of a single customer record.
- Allow controlled removal of a single customer record.

## Requirements

### 1. Customer list

- [P0] Sale Master can view all available customers.
- [P0] The list must show at least: name, status, and connected channels.
- [P0] Connected channels may include Zalo, Email, Phone, Facebook, and other supported channels.
- [P0] The list should support a safe empty state.

### 2. Customer detail editing

- [P0] Sale Master can open one customer record and edit its details.
- [P0] Editable fields include at least: name, phone, email, status, and tax information.
- [P0] The system must validate updates before saving.
- [P0] The system must keep audit history for changes.

### 3. Customer removal

- [P0] Sale Master can remove a single customer record when permitted.
- [P0] The system must confirm the action before deletion.
- [P0] The system must prevent accidental bulk removal through this feature.
- [P0] Removal must be auditable.

## Cross-links

- Related sourcing feature: [Rocket Agents / Customer Pool Generating](/products/bplatform-rocket-agents/features/01-customer-pool-generating.md)

## Success Measures

- Time required to locate and update a customer.
- Reduction in duplicate or stale customer records.
- Successful completion rate for customer edits and removals.
- Audit completeness for customer changes.
