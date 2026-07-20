# Authentication

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.19 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Figma** | [Design](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9?node-id=90-2) |

| **Tracklogs** | [Multica BPL-246](https://tasks.tvlong.me/issues/BPL-246) |
---

## The Problem

Registered business customers need secure, reliable access to their accounts to view quotes, place orders, and manage their business information. Weak or cumbersome authentication risks both security and abandonment.

- Customers need secure login to access account-specific data (quotes, orders).
- Customers may belong to one or multiple Business Units and must work in the right company context after login.
- Session management must balance security and convenience.
- Repeated failed login attempts must be controlled to protect customer accounts from brute-force attacks.

## Proposed Solution

An **Authentication** system providing secure credential verification, Business Unit selection, return-to-previous-page behavior, logout, and session management.

### Goals

- Secure account access with industry-standard practices.
- Smooth login/logout experience with correct post-login redirection.
- Ensure users work under the right Business Unit after authentication.

### Out-of-scope

- OTP gateway for Enterprise users (next version).
- Optional OTP opt-in for Individual Business users (next version).
- SSO / social login (future).

### Measurable Outcomes

- Login success rate.
- Login failure / lockout rate.
- Business Unit selection completion rate.
- Auth-related support tickets.

## Requirements

### 9.1 User provide wrong username or password

- [P0] User can submit Email/Phone and Password for credential verification.
- [P0] System uses Cloudflare automatic CAPTCHA / bot protection before accepting authentication submission to prevent bot login attempts.
- [P0] If Email/Phone or Password is incorrect, system shows a general error: `Username or Password is wrong`.
- [P0] System must not reveal whether the Email/Phone exists.
- [P0] System counts failed password submissions per user account.
- [P0] If user submits wrong password 5 times within 1 hour, system blocks the user from authentication for 1 hour since the last failed submission.
- [P0] During lockout period, system prevents credential verification attempts for the blocked user.

### 9.2 User provide correct credential

- [P0] User can submit valid Email/Phone and Password.
- [P0] If credential verification succeeds, system continues to Business Unit resolution.
- [P0] System creates a JWT-based session after successful authentication and Business Unit resolution.
- [P0] System redirects the user after successful authentication according to **9.4 User authenticates from any page**.
- [P0] User can securely logout; logout invalidates the active session.
- [P1] User can select "Remember me" during login.

#### 9.2.1 User belongs to single BU (Business Unit)

- [P0] If user belongs to a single Business Unit, system automatically selects that Business Unit.
- [P0] System redirects the user after selecting the Business Unit automatically according to **9.4 User authenticates from any page**.
- [P0] The active session stores the selected Business Unit context.

#### 9.2.2 User belongs to multiple BUs

- [P0] If user belongs to multiple Business Units, system shows a Company / Business Unit selection step.
- [P0] User can choose which Company / Business Unit they would like to work on.
- [P0] After selection, system redirects the user according to **9.4 User authenticates from any page**.
- [P0] The active session stores the selected Business Unit context.

### 9.3 User enable 2FA (2 factor-authentication)

- [P1] After successful credential verification, system can require an OTP as a final authentication gateway.
- [P1] Enterprise Business users must complete 2FA before Business Unit resolution. _(Next version)_
- [P2] Individual Business users can opt in to 2FA; when enabled, OTP is required after credential verification. _(Next version)_
- [P2] OTP verification must complete before session creation. _(Next version)_

#### 9.3.1 User receive OTP via Email

- [P1] System sends a 6-digit OTP to the user's email after credential verification succeeds. _(Next version)_
- [P1] User can submit the 6-digit OTP to complete authentication. _(Next version)_
- [P2] OTP codes expire after a short period. _(Next version)_
- [P2] System applies resend limits for OTP emails. _(Next version)_

### 9.4 User authenticates from any page

- [P0] If authentication is opened from the Home Page, redirect the user back to the Home Page after successful authentication and Business Unit resolution.
- [P0] If authentication is opened via a referral link, redirect the user to the Home Page after successful authentication and Business Unit resolution.
- [P0] If authentication is opened from any other page, preserve the originating page URL and redirect the user back to that originating page after successful authentication and Business Unit resolution.
- [P0] If the originating page URL is unavailable or invalid, fall back to the Home Page after successful authentication and Business Unit resolution.
- [P0] If authentication is triggered from the Cart page while the user is creating a quote request, recover the temporarily stored quote-request information and continue the **Request a Quote** process directly after successful authentication; do not redirect the user back to the Cart page.
- [P0] If the user has items in the cart before authentication and does not have cart items from a previous authenticated session, preserve all cart items after successful authentication.
- [P0] If the user has items in the cart before authentication and also has cart items from a previous authenticated session, show a cart-resolution prompt after successful authentication.
	- [P0] If the user chooses **Overwrite with new Cart**, replace the previous authenticated-session cart with the current local cart.
	- [P0] If the user chooses **Use old cart only**, discard the current local cart and restore the previous authenticated-session cart.
	- [P0] If the user chooses **Merge both cart items**, combine the current local cart and previous authenticated-session cart into one cart.
- [P0] If the user has items in the cart after authentication and then logs out, clear the cart from local state because the cart now belongs to the previous authenticated user.

## Appendix

### Business Unit Selection

- **Single Business Unit** — The system automatically selects the user's only Business Unit and redirects according to **9.4 User authenticates from any page**.
- **Multiple Business Units** — The system shows a Company / Business Unit selection step. After selection, the user is redirected according to **9.4 User authenticates from any page**.

### Lockout Rule

- A failed password submission is counted per user account.
- When the user reaches 5 failed password submissions within 1 hour, authentication is blocked for 1 hour from the latest failed submission.
- The lockout applies before credential verification to avoid repeated brute-force attempts during the blocked period.
