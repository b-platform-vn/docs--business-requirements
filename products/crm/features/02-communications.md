# Communications

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.09.02 |
| **Product** | [CRM](/products/crm/README.md) |
| **Priority** | P1 |

---

## The Problem

Sale Master needs to communicate with connected customers across all supported channels and review the full history of a customer conversation in one place. Without a dedicated Communications capability, follow-up is slow and channel history is fragmented.

## Proposed Solution

A **Communications** capability that lets Sale Master send messages to any connected customer through any connected channel and review the full conversation history for a single customer.

### Goals

- Send outbound messages to connected customers through the right channel.
- Show all conversation history for a single customer across channels.
- Preserve context so sales follow-up can continue smoothly.
- Keep every action auditable.

## Requirements

### 1. Send a message

- [P0] Sale Master can send a message to any connected customer.
- [P0] Sale Master can choose any connected channel available for that customer.
- [P0] The system must route the message through the appropriate connector/service.
- [P0] The system must show send success or failure.
- [P0] Every send must be auditable.

### 2. View conversation history

- [P0] Sale Master can open a single customer and see all conversation histories.
- [P0] The history must aggregate messages across all connected channels.
- [P0] The history must preserve sender, channel, timestamp, and content.
- [P0] The view should support a safe empty state when no messages exist.

## Success Measures

- Message send completion rate.
- Time to find conversation history.
- Coverage of conversation history across supported channels.
- Audit completeness for outbound messages.
