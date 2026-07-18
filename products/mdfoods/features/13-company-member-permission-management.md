# Company Member/Permission Management

| | |
|---|---|
| **Author** | _TBD_ |
| **Updated** | 2026.07.17 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P1 |

---

## The Problem

Enterprise Business customers have multiple people involved in purchasing — buyers, approvers, and viewers. They need to invite team members and control what each can do (place orders, request quotes, view only).

- Multiple employees purchase on behalf of one company.
- Different roles need different permissions (approve vs. request vs. view).
- Account owners need control over team access.

## Proposed Solution

A **Company Member/Permission Management** section for Enterprise accounts to invite members, assign roles, and control permissions.

### Goals

- Enable team-based purchasing under one company account.
- Enforce role-based access control.
- Give account owners control over member access.

### Out-of-scope

- Approval workflow engine (future — may be separate feature).
- Individual Business accounts (single user).

### Measurable Outcomes

- Members invited per Enterprise account.
- Permission-related support tickets.

## Requirements

### Member Management

- [P0] Invite members by email.
- [P0] View list of company members and their roles.
- [P0] Remove members.
- [P1] Resend / revoke pending invitations.

### Roles & Permissions

- [P0] Assign roles: Admin, Purchaser, Viewer.
- [P0] Enforce permissions across ordering, quoting, and viewing.
- [P1] Custom permission granularity.

### Role Definitions

| Role | Permissions |
|---|---|
| Admin | Full access: manage members, orders, quotes, company info |
| Purchaser | Create quotes, place orders, manage cart |
| Viewer | View orders, quotes, and products only |

## Appendix

_None yet._
