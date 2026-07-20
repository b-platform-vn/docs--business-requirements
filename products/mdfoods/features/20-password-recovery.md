# Password Recovery

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.19 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Figma Design** | [Password Recovery flow](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9/MDFoods.vn?node-id=246-6664&t=lmZ9cfhbP73agc81-4) |
| **Tracklogs** | [Multica BPL-257](https://tasks.tvlong.me/issues/BPL-257) |

---

## The Problem

Registered business customers may forget their password or lose access to their account. Without a reliable recovery flow, customers cannot access quotes, orders, and business information, which increases support workload and blocks transactions.

- Customers need a secure way to recover account access.
- Recovery must confirm ownership without exposing account existence.
- Recovery tokens must be short-lived and protected from reuse.

## Proposed Solution

A **Password Recovery** flow that allows users to request a password reset link by Email/Phone, verify a secure recovery token, and set a new password.

### Goals

- Help registered users regain account access securely.
- Reduce support requests related to forgotten passwords.
- Protect customer accounts from token replay and enumeration attacks.

### Out-of-scope

- Account recovery through customer support agent approval.
- Recovery using social login / SSO.
- OTP-based passwordless login.

### Measurable Outcomes

- Password reset request rate.
- Password reset completion rate.
- Password recovery failure rate.
- Auth-related support tickets.

## Requirements

### 20.1 User request password recovery

- [P0] User can access a `Forgot password` action from the login page.
- [P0] User can submit Email/Phone to request password recovery.
- [P0] System uses Cloudflare automatic CAPTCHA / bot protection before accepting password recovery submission to prevent bot requests.
- [P0] System always shows a general confirmation message and does not reveal whether the Email/Phone exists.
- [P0] If the account exists, system sends a password reset link to the user's registered email.
- [P1] System applies rate limiting to password recovery requests.

### 20.2 User open password reset link

- [P0] Password reset link contains a secure token.
- [P0] Password reset token has an expiry time.
- [P0] System rejects expired, invalid, or already-used reset tokens.
- [P0] System shows a clear error state when the reset link is invalid or expired.

### 20.3 User set new password

- [P0] User can enter a new password after opening a valid reset link.
- [P0] New password must satisfy password strength requirements.
- [P0] User must confirm the new password before submission.
- [P0] System updates the password after successful validation.
- [P0] System invalidates the reset token after successful password update.
- [P1] System invalidates existing active sessions after password reset.

### 20.4 User change password from profile

- [P1] Authenticated user can change password from profile settings.
- [P1] User must provide current password before setting a new password.
- [P1] New password must satisfy password strength requirements.
- [P1] System confirms successful password change.

## Appendix

### Security Notes

- Password recovery responses must avoid account enumeration.
- Reset tokens must be single-use and time-limited.
- Reset tokens must not be stored in plaintext.
- Password reset events should be auditable for security review.
