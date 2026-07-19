# Registration

| | |
|---|---|
| **Author** | annguyen1 |
| **Updated** | 2026.07.19 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Figma** | [Design](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9?node-id=89-2) |

---

## The Problem

To place bulk orders and receive quotes, business customers must have accounts. The platform serves two distinct customer types — Individual Business and Enterprise Business — each with different registration needs.

- Bulk ordering and quoting require an authenticated business account.
- Individual and Enterprise customers have different data requirements.
- Registration must create a corresponding customer record in the CRM.
- MDFoods needs a structured first touchpoint for large enterprises and family-run businesses to register interest and start onboarding.
- Duplicate business identifiers, especially Tax ID, must route users into the right registration or join-company journey.

MDFoods also needs a simple way for large enterprises and family-run businesses to express interest and register to join the platform. Without a clear sign-up step, MDFoods risks losing potential B2B customers due to the lack of a structured process for collecting basic business contact information and initiating onboarding.

This feature creates the first touchpoint for businesses to register their interest with MDFoods.

## Proposed Solution

MDFoods will provide a single, unified sign-up process for both large enterprises and family-run businesses. The form will collect mandatory information about the business and the registrant to initiate verification, including business contact information, business type, Tax ID, and the registrant's authority/role.

For family-run businesses without an official brand name, the registrant's full name may be used as the Business Name.

The **Registration** flow supports both Individual Business and Enterprise Business account types and integrates with the CRM to create customer records.

### Goals

- Enable business customers to self-register.
- Capture the right data per customer type.
- Sync new customers to the CRM.
- Allow both large enterprises and family-run businesses to submit registration requests through the same unified process.
- Ensure every registration request includes the minimum information MDFoods needs to identify and contact the business.
- Clearly distinguish business type, Tax ID, and registrant authority so the subsequent verification step does not rely on assumptions.
- Keep the sign-up process simple enough for small family-run businesses while still fully supporting the needs of large enterprises.

### Out-of-scope

- Social login / SSO (future).
- Guest checkout (B2B requires accounts).

### Measurable Outcomes

- Registration completion rate.
- Drop-off rate per registration step.
- Duplicate account / company detection rate.
- Join-company request completion rate.
- Quote-request registration resume success rate.

## Requirements

### 8.1 New user registration

- [P0] If the user enters a duplicate email or phone number, show an error that the email or phone number already exists.
- [P0] If the user enters a duplicate Tax ID, redirect to **8.3 User requests to join a company already registered by someone else**.
- [P0] If the user leaves mandatory fields blank, show missing required field errors and mark the registration as incomplete.
- [P0] Even though Email OTP verification is not included in the current scope, reserve a user-level column to indicate whether the user's email has been verified.
- [P0] If all mandatory fields are valid, registration is successful; redirect the account to the profile page or home page while pending approval.

### 8.2 User registers an additional company

Triggered from the company management/account information page by clicking **Add new company**.

- [P0] If the user enters a Tax ID that duplicates a company already existing in the system and not owned by the user, redirect to **8.3 User requests to join a company already registered by someone else**.
- [P0] If the user enters a Tax ID that already belongs to one of their own existing companies, show a duplicate company error and do not create a new company record.
- [P0] If the user enters a new, non-duplicate Tax ID and all mandatory information is valid, create a new company linked to the current user without affecting previously linked companies; move the company to pending-approval status.
- [P0] If mandatory information is missing, show an incomplete-information error and do not proceed.

### 8.3 User requests to join a company already registered by someone else

- [P0] If the requested company has not yet been activated, show a notice and hold the join request until the company is approved.
- [P0] If the requested company has already been activated, send the join request to the company's Admin for approval.
- [P0] If the user is already a member of the requested company, show an error and do not allow duplicate requests.
- [P0] If the user is already linked to another company, still allow the join request; the user's relationship with their current company remains unchanged.

### 8.4 User registers from any page

- [P0] If registration is opened from the Home Page, redirect the user back to the Home Page after successful registration.
- [P0] If registration is opened via a referral link, redirect the user to the Home Page after successful registration.
- [P0] If registration is opened from any other page, preserve the originating page URL and redirect the user back to that originating page after successful registration.
- [P0] If the originating page URL is unavailable or invalid, fall back to the Home Page after successful registration.
- [P0] If the user has items in the cart before registration, preserve all cart items after successful registration.
- [P0] If the user has items in the cart after registration and then logs out, clear the cart from local state because the cart now belongs to the previous authenticated user.
- [P0] If registration is triggered while the user is already linked to a company, redirect to **8.2** to register another company, or **8.3** to join an existing company.

### 8.5 User registers while creating a quote request

- [P0] If a guest clicks **Request a Quote** from the Cart page, automatically trigger registration because quote requests require authentication.
- [P0] Before triggering registration, temporarily store all filled quote-request information so the user does not need to re-enter it after registration.
- [P0] If registration completes successfully, recover all temporarily stored quote-request information and continue the **Request a Quote** process directly.
- [P0] If registration completes successfully from the quote-request flow, do not redirect the user back to the Cart page.
- [P0] If the user chooses to log in instead of registering, refer to [Authentication](09-authentication.md) and continue based on the authenticated login flow.
- [P0] If the user abandons the registration process midway, clear the temporarily stored quote-request information.
- [P0] If the user abandons the registration process midway, keep the Cart state unchanged.

### 8.6 User verifies registration via Email OTP

- [P1] Next version: Email OTP verification is not included in the current scope, but the current implementation must keep the reserved email-verification column ready for future use.
- [P1] Next version: after the user submits valid registration information, send an OTP to the registered email address.
- [P1] Next version: require the user to enter the correct Email OTP before completing registration submission.
- [P1] Next version: if the Email OTP is incorrect or expired, show an error and allow the user to request a new OTP.
- [P1] Next version: if the user does not complete Email OTP verification, keep the registration incomplete and do not submit it for approval.

### Validation

- [P0] Email uniqueness and format validation.
- [P0] Password strength requirements.
- [P0] Duplicate Tax ID detection.
- [P0] Mandatory business contact information, business type, Tax ID, and registrant authority/role validation.
- [P1] Tax ID / company validation for Enterprise.

## Appendix

### Customer Types

- **Individual Business** — Sole proprietors or small buyers; lighter data requirements.
- **Enterprise Business** — Larger organizations requiring company info and team member management.
