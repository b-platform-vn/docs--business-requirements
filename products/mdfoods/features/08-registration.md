# Registration

| | |
|---|---|
| **Author** | _TBD_ |
| **Updated** | 2026.07.18 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Figma** | [Design](https://www.figma.com/design/LhJxbmnpQ1D7CY3SIvyCU9?node-id=89-2) |

---

## The Problem

To place bulk orders and receive quotes, business customers must have accounts. The platform serves two distinct customer types — Individual Business and Enterprise Business — each with different registration needs.

- Bulk ordering and quoting require an authenticated business account.
- Individual and Enterprise customers have different data requirements.
- Registration must create a corresponding customer record in the CRM.

MDFoods also needs a simple way for large enterprises and family-run businesses to express interest and register to join the platform. Without a clear sign-up step, MDFoods risks losing potential B2B customers due to the lack of a structured process for collecting basic business contact information and initiating onboarding.

This feature creates the first touchpoint for businesses to register their interest with MDFoods.

## Target Users

### Target Users

- **Large Enterprises / Corporations** — Restaurants, hotels, F&B chains, industrial kitchens, schools, or similar corporate buyers.
- **Family-run Businesses** — Small food shops, bakeries, family restaurants, or individual household businesses.

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

## Requirements

### Registration Flow

- [P0] Select customer type: Individual Business or Enterprise Business.
- [P0] Collect basic account details (name, email, password, phone).
- [P0] Collect business contact information, business type, Tax ID, and registrant authority/role.
- [P0] Enterprise: collect company information (name, tax ID).
- [P0] Family-run businesses without an official brand name may use the registrant's full name as the Business Name.
- [P0] Create customer record in CRM on successful registration.
- [P0] Email verification.
- [P1] Terms and conditions acceptance.
- [P2] Progressive/multi-step form with save progress.

### Use Cases

| Use Case | Primary Actor | Business Need | Expected Business Outcome |
|---|---|---|---|
| Enterprise sign-up | Large Enterprise / Corporation | Submit company information to start MDFoods' verification process. | MDFoods receives the company's registration request with full contact information and Tax ID. |
| Family-run business sign-up | Family-run Business / Sole Proprietor | Register interest even without an official brand name. | MDFoods receives the household business registration; the registrant's name may be used as the Business Name. |
| Sign-up via authorized registrant | Authorized Registrant | Submit registration information on behalf of the business. | MDFoods knows exactly who submitted the request and their relationship to the business. |
| New business sign-up | New business applicant | Start the verification process for a business that has never existed on MDFoods. | A new business registration request is created. |
| Existing user registers an additional business | Existing MDFoods user | Register a second business without losing access to the existing business. | A separate business registration request is created, and the relationship with the existing company is preserved. |
| User already linked to a company registers another business | Existing user of a company | Request verification for another company while retaining access to the current company. | The new registration request is processed independently of the user's current company relationship. |
| Existing business detected | MDFoods / Applicant | Avoid creating duplicate business registration records. | A match on Tax ID, business email, or phone number is flagged as a suspected existing business. |
| Incomplete registration | Applicant / MDFoods | Prevent incomplete information from entering the verification process. | The request cannot proceed until mandatory information is fully provided or corrected. |
| Pending verification status | Applicant / MDFoods | Prevent the business from purchasing before the review process is complete. | The business remains in pending-approval status and has not been granted purchasing rights. |
| Switching business entity after login | User linked to multiple businesses | Choose which business entity to act on behalf of. | Registration and verification status is displayed according to the selected business entity. |
| Authentication gate at checkout | Guest user | Only proceed to checkout after registering or logging in. | The user must authenticate before completing checkout. |
| Redirect to checkout after sign-up | User who registered from the checkout flow | Resume the interrupted checkout journey after completing registration. | The user is redirected back to the checkout flow after successful authentication. |

### Mandatory Requirements

#### 5.1 New user registration

- [P0] If the user enters a duplicate email or phone number, show an error that the email or phone number already exists.
- [P0] If the user enters a duplicate Tax ID, redirect to **5.3 User requests to join a company already registered by someone else**.
- [P0] If the user leaves mandatory fields blank, show missing required field errors and mark the registration as incomplete.
- [P0] If all mandatory fields are valid, registration is successful; redirect the account to the profile page or home page while pending approval.

#### 5.2 User registers an additional company

##### 5.2.a Logged-in user

Triggered from the company management/account information page by clicking **Add new company**.

- [P0] If the user enters a Tax ID that duplicates a company already existing in the system and not owned by the user, redirect to **5.3 User requests to join a company already registered by someone else**.
- [P0] If the user enters a Tax ID that already belongs to one of their own existing companies, show a duplicate company error and do not create a new company record.
- [P0] If the user enters a new, non-duplicate Tax ID and all mandatory information is valid, create a new company linked to the current user without affecting previously linked companies; move the company to pending-approval status.
- [P0] If mandatory information is missing, show an incomplete-information error and do not proceed.

##### 5.2.b Logged-out user

Example: the user already has an account and one company on another device/session, and is now registering a new company on the current device/session while logged out.

- [P0] If the user is not linked to any company because the account does not exist, this is not applicable; redirect to **5.1 New user registration**.
- [P0] If the personal information (email/phone number) matches an existing account, show a confirmation prompt: “This account already exists — would you like to register an additional company?”
	- If the user confirms, log the user in and continue under **5.2.a Logged-in user** to complete the new company creation.
	- If the user declines, stop the process and keep the user logged out.
- [P0] If the personal information (email/phone number) does not match any existing account, this is a completely new user; redirect to **5.1 New user registration**.

#### 5.3 User requests to join a company already registered by someone else

- [P0] If the requested company has not yet been activated, show a notice and hold the join request until the company is approved.
- [P0] If the requested company has already been activated, send the join request to the company's Admin for approval.
- [P0] If the user is already a member of the requested company, show an error and do not allow duplicate requests.
- [P0] If the user is already linked to another company, still allow the join request; the user's relationship with their current company remains unchanged.

#### 5.4 User registers from any page

- [P0] If registration is triggered from a page other than the sign-up page, redirect to the standard sign-up flow: **5.1**, **5.2**, or **5.3**, depending on the user's context.
- [P0] If registration is triggered while the user is already linked to a company, redirect to **5.2** to register another company, or **5.3** to join an existing company.
- [P0] If registration is triggered via an invite/join link, redirect to **5.3**.
- [P0] If registration is triggered from a page with no available context, fall back to the default standard sign-up flow.
- [P0] If session/context data from the originating page is preserved, return to the correct sign-up journey after successful submission.
- [P0] If session/context data from the originating page is lost, use the default sign-up flow.
- [P0] If the user is already logged in, continue the registration process under the current account's context.
- [P0] If the user is not logged in, show a prompt to register or log in, then continue on the correct branch of the process.

#### 5.5 User registers while creating a quote request

- [P0] If the user is not logged in and starts creating a quote request, require registration or login before proceeding; temporarily preserve the quote request data.
- [P0] If registration completes successfully, resume and complete the quote request using the previously preserved data.
- [P0] If the user chooses to log in instead of registering, continue the quote request using the existing account.
- [P0] If the user abandons the registration process midway, discard the quote request data or expire it after a timeout period.
- [P0] If registration fails due to missing information, keep the quote request in temporary storage until the user corrects the registration information or actively cancels it.
- [P0] If registration fails due to a duplicate email/phone number, stop the process and require the user to resolve the account conflict before continuing.
- [P0] If registration fails due to a duplicate Tax ID, redirect to **5.3** if the quote request is tied to joining an existing company.
- [P0] If the quote data conflicts with the selected business entity, show a clarification/re-selection step before completing the quote.
- [P0] If a quote request is already active within the same session, continue the current quote session instead of creating a duplicate.

### Validation

- [P0] Email uniqueness and format validation.
- [P0] Password strength requirements.
- [P0] Duplicate Tax ID detection.
- [P0] Mandatory business contact information, business type, Tax ID, and registrant authority/role validation.
- [P1] Tax ID / company validation for Enterprise.

### Important but not MVP-blocking Requirements

- [P1] The sign-up flow should clearly communicate that submitting the form only starts the verification/onboarding process and does not immediately grant purchasing rights.
- [P1] MDFoods needs to clearly define how to handle duplicate cases for Tax ID, business email, or phone number.
- [P1] MDFoods needs to determine whether an authorized registrant should be treated differently from the business owner during the later, in-depth verification step.
- [P2] Email/Phone OTP verification during sign-up is not currently a technical requirement; it can be handled temporarily via a manual confirmation email, but should be considered for automation later.

## Appendix

### Customer Types

- **Individual Business** — Sole proprietors or small buyers; lighter data requirements.
- **Enterprise Business** — Larger organizations requiring company info and team member management.
