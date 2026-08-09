# Company Information Management

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.08.05 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P0 |
| **Tracklogs** | [Multica BPL-249](https://tasks.tvlong.me/issues/BPL-249) |

---

## The Problem

Authenticated business customers can be linked to one or multiple companies through [Registration](08-registration.md). MDFoods must let them view and maintain the correct business entity while keeping company data separate from the user's personal profile and authentication identity.

- B2B invoicing requires accurate company name, Tax ID, registration information, and business documents.
- A user may register an additional company or request to join an existing company without affecting their current company relationships.
- A user linked to multiple companies must know which Company / Business Unit is active for quotes, orders, and invoices.
- Personal profile changes must not modify company information, and company changes must not modify the user's authentication identity or personal profile.
- Company information and uploaded documents must only be visible and editable within the correct authorized company context.

## Proposed Solution

An authenticated **Company Information Management** section where users can view all companies linked to their account, inspect company status and business details, switch their active Company / Business Unit, and update company information when authorized.

This feature is the management surface for company records after registration. Registering an additional company and requesting to join an existing company continue through the workflows defined in [Registration](08-registration.md). Selecting and storing the active Business Unit follows [Authentication](09-authentication.md). Personal account fields remain managed by [Profile Management](10-profile-management.md).

### Goals

- Allow users to view every company linked to their account and each company's current status.
- Allow authorized users to maintain accurate company information and business registration documents.
- Provide entry points for registering another company or requesting to join an existing company.
- Keep the active Company / Business Unit context consistent with the authenticated session.
- Keep company information separate from personal profile and authentication data.
- Supply accurate company information to quotes, orders, and invoices.
- Reduce support requests for company information changes.

### Out-of-scope

- New user registration, additional-company creation rules, duplicate Tax ID resolution, and join-company approval; these are defined in [Registration](08-registration.md).
- Credential verification, session creation, and initial Business Unit resolution; these are defined in [Authentication](09-authentication.md).
- Personal profile fields such as Fullname, Gender, Phone, Birthday, and account Email; these are defined in [Profile Management](10-profile-management.md).
- Company member invitations, roles, and permission assignment; these are defined in [Company Member/Permission Management](13-company-member-permission-management.md).
- Shipping and billing address-book management; this is defined in [Addresses Management](11-addresses-management.md).
- Company verification / KYB decision processing; this feature only displays the resulting company status.
- Invoice generation, payment, and invoice download.

### Measurable Outcomes

- Company information completion rate.
- Company information edit success and validation-failure rates.
- Additional-company and join-company flow completion rates from this section.
- Business Unit switch success rate.
- Business registration document upload success rate.
- Reduction in support tickets for company information changes.

## Requirements

### 12.1 User views linked companies

- [P0] User must be authenticated to access Company Information Management.
- [P0] System shows every company linked to the authenticated user without removing or replacing any existing company relationship.
- [P0] For each linked company, system shows company name, business type, company status, and whether it is the active Company / Business Unit.
- [P0] System distinguishes at minimum between **Pending approval** and **Active** company statuses.
- [P0] A company awaiting approval remains visible to the user but cannot be used as the active Business Unit until it becomes active.
- [P0] If the user is linked to no company, system shows an empty state with actions to **Register another company** or **Join an existing company** according to [Registration](08-registration.md).
- [P0] If linked companies cannot be loaded, system shows a general error and allows user to retry.
- [P0] System must not expose companies that are not linked to the authenticated user.

### 12.2 User views company information

- [P0] User can open a linked company and view its current company information.
- [P0] System shows company name, business type, Tax ID, registration number, business phone, business email, and registered business address when available for that company type.
- [P0] System shows the company's current approval status.
- [P0] System shows uploaded business registration documents and each document's current upload or review status.
- [P0] System shows whether the company is the active Company / Business Unit in the current authenticated session.
- [P1] System shows the company logo when one has been uploaded.
- [P1] System shows separate billing-entity information when it differs from the company information.

### 12.3 User registers or joins another company

- [P0] Company Information Management provides a **Register another company** action that starts **8.2 User registers an additional company** in [Registration](08-registration.md).
- [P0] Company Information Management provides a **Join an existing company** action that starts **8.3 User requests to join a company already registered by someone else** in [Registration](08-registration.md).
- [P0] Registering or requesting to join another company must not remove, replace, or modify the user's existing company relationships.
- [P0] If an additional-company registration succeeds, system returns user to Company Information Management and shows the new company with **Pending approval** status.
- [P0] If a join-company request is submitted, system returns user to Company Information Management and shows the request outcome defined by **8.3**.
- [P0] Duplicate Tax ID detection and routing must follow **8.2** and **8.3**; Company Information Management must not create a duplicate company record.
- [P0] A newly registered or joined company must not automatically replace the active Business Unit while it is pending approval.

### 12.4 User switches active Company / Business Unit

- [P0] If the user is linked to multiple active companies, system allows user to choose which Company / Business Unit to work on.
- [P0] Only a company with **Active** status and an active user membership can be selected as the active Business Unit.
- [P0] After selection succeeds, system updates the Business Unit stored in the authenticated session according to [Authentication](09-authentication.md).
- [P0] System clearly identifies the selected Business Unit throughout authenticated, company-scoped experiences.
- [P0] Switching the active Business Unit must not remove or modify any company membership.
- [P0] Switching the active Business Unit must not expose quotes, orders, invoices, members, or other company-scoped data from the previously active Business Unit.
- [P0] If switching fails, system keeps the previous Business Unit active and shows an error that allows user to retry.
- [P0] A **primary company** preference, if introduced later, must not be used as a substitute for the active Business Unit stored in the authenticated session.

### 12.5 Authorized user edits company information

- [P0] Only a user with permission to manage company information can enter edit mode or submit company changes.
- [P0] Authorization must be evaluated against the company being edited, not only against the user's global account.
- [P0] An authorized user can edit company name, business type, Tax ID, registration number, business phone, business email, and registered business address according to applicable company rules.
- [P0] System validates submitted company information before saving.
- [P0] If validation succeeds, system saves the changes and shows a success message with the latest company values.
- [P0] If saving fails, system shows a general error and keeps the user's entered values on the screen.
- [P0] Editing one company must not modify another linked company.
- [P0] Editing company information must not modify the user's personal profile fields, account Email, authentication identity, or company memberships.
- [P1] If user cancels editing, system discards unsaved changes and restores the last saved company values.
- [P1] If user navigates away with unsaved changes, system should warn user before discarding changes.
- [P1] System records an audit history of company information changes, including actor and timestamp.

### 12.6 Authorized user manages business registration documents

- [P0] An authorized user can upload business registration documents for the selected company.
- [P0] System validates each document against the supported file types and maximum file size before uploading.
- [P0] If document validation or upload fails, system shows a specific error and allows user to retry.
- [P0] If upload succeeds, system associates the document only with the selected company and shows the latest document status.
- [P0] System must not expose a company's documents to users who cannot access that company.
- [P1] An authorized user can replace an outdated business registration document without changing historical order or invoice records.

### 12.7 Company information supplies invoicing data

- [P0] When a quote or order is created under an active Business Unit, system uses that company's applicable invoicing information.
- [P0] An order invoice stores a snapshot of the company and billing information used when the invoice is generated.
- [P0] Later company information changes must not retroactively change historical invoices.
- [P0] System must not populate an invoice with information from a different linked company or previously active Business Unit.
- [P1] An authorized user can maintain a separate billing entity when billing information differs from the selected company.

### 12.8 Company information remains separate from personal profile

- [P0] Company Information Management must not provide controls for editing Fullname, Gender, Phone, Birthday, or account Email defined in [Profile Management](10-profile-management.md).
- [P0] Updating a user's personal Phone in Profile Management must not automatically overwrite the company's business phone.
- [P0] Updating a company's business phone or business email must not automatically overwrite the user's personal Phone or account Email.
- [P0] Updating company information must not change authentication credentials or invalidate the authenticated session unless access to the active Business Unit is revoked.
- [P0] Profile updates must not change the active Business Unit or any company relationship.

### 12.9 Company information validation and authorization

- [P0] Company name, business type, Tax ID, business contact information, and registrant authority/role required by [Registration](08-registration.md) must remain valid for the applicable customer and company type.
- [P0] Enterprise Business company information requires a valid registration number and required business registration documents.
- [P0] Company name and text fields must not be empty after trimming whitespace.
- [P0] Business email must use a valid email format.
- [P0] Business phone must use a valid phone-number format accepted by MDFoods.
- [P0] Tax ID and registration number must use formats accepted by MDFoods for the company's jurisdiction.
- [P0] Tax ID uniqueness and duplicate-company handling must follow [Registration](08-registration.md).
- [P0] If any validation fails, system shows field-level validation messages and prevents saving.
- [P0] System enforces company access and edit permissions on the server for every read, update, Business Unit switch, and document request.
- [P0] Hiding an edit control in the interface must not be treated as authorization enforcement.

## Appendix

### Domain Definitions

| Term | Definition |
|---|---|
| User | The personal MDFoods account and profile described in [Profile Management](10-profile-management.md). |
| Company | A business entity registered with MDFoods and identified by company information such as company name and Tax ID. |
| Business Unit | The company context selected for the authenticated session as described in [Authentication](09-authentication.md). In the current scope, a selectable Business Unit corresponds to an active company linked to the user. |
| Company membership | The relationship between a user and a company, including role and access status. Membership permissions are described in [Company Member/Permission Management](13-company-member-permission-management.md). |
| Pending approval | The company has been submitted but cannot yet be selected as the active Business Unit. |
| Active | The company has been approved and may be selected as the active Business Unit by an active member. |

### Cross-feature Ownership

| Concern | Source of truth |
|---|---|
| New user registration | [Feature 8 — Registration](08-registration.md) |
| Register another company and duplicate Tax ID handling | [Feature 8 — Registration](08-registration.md), sections 8.2 and 8.3 |
| Login, session creation, and initial Business Unit resolution | [Feature 9 — Authentication](09-authentication.md) |
| Personal account information | [Feature 10 — Profile Management](10-profile-management.md) |
| Linked-company display, active Business Unit switching, company edits, and business documents | Feature 12 — Company Information Management |
| Company membership roles and permissions | [Feature 13 — Company Member/Permission Management](13-company-member-permission-management.md) |

### Company and Profile Separation Rule

- Personal account data belongs to the User and is shared across the user's MDFoods account.
- Company data belongs to one Company / Business Unit and is shared only with authorized company members.
- The same user can be linked to multiple companies without duplicating the user's personal profile.
- No personal profile update may implicitly update company data, and no company update may implicitly update personal profile or authentication data.
