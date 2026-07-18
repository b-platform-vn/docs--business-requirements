# In-App Messages and Notification

| | |
|---|---|
| **Author** | _TBD_ |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P1 |

---

## The Problem

Business customers need timely communication about quote responses, order updates, and support — without leaving the platform or relying solely on email. MDFoods depends on the CRM's Communication Services for messaging capabilities.

- Customers need real-time updates on quotes and orders.
- Support conversations should happen in-context.
- Email alone is slow and easily missed.

## Proposed Solution

An **In-App Messages and Notification** feature integrating the CRM Communication Services to deliver real-time messages and notifications within the MDFoods web app.

### Goals

- Deliver timely, in-context communication.
- Centralize customer support messaging via CRM.
- Reduce reliance on email for critical updates.

### Out-of-scope

- Building a new messaging backend (uses CRM Communication Services).
- SMS notifications (future).

### Measurable Outcomes

- Message open / response rate.
- Notification-driven engagement (quote acceptance, order actions).
- Support resolution time via in-app chat.

## Requirements

### In-App Messages

- [P0] Display message inbox within the account area.
- [P0] Real-time send/receive via CRM Communication Services ↔ FrontEnd (NextJS).
- [P0] Support conversation threads (support chat).
- [P1] Unread message indicators.

### Notifications

- [P0] In-app notifications for quote responses and order updates.
- [P0] Notification center / bell icon in header.
- [P1] Browser push notifications.
- [P2] Notification preferences (opt in/out per type).

## Appendix

### Integration Note

This feature relies on the [CRM](/products/crm/) Communication Services. See the [Stack / Tools](/products/mdfoods/stack.md) page — CRM Integration section — for the architecture:

```
[CRM - Communication Services] <-- Send/Receive Msg --> [FrontEnd - NextJS]
```
