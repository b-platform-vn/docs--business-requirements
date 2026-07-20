# Profile Management

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.20 |
| **Product** | [MDFoods](/products/mdfoods/README.md) |
| **Priority** | P1 |

---

## The Problem

Business customers need to view and update their personal account information without contacting support, while MDFoods must preserve stable account identity for authentication, quote history, orders, and company membership.

- Account details such as full name, gender, phone, and birthday can change over time.
- Email is used to identify the user account and must remain stable in the profile screen.
- Self-service profile updates reduce support requests.
- Profile changes must not affect Business Unit membership, company information, quote history, or authentication identity.

## Proposed Solution

An authenticated **Profile Management** section where users can view their profile information and update editable personal fields. Email is displayed as a read-only identity field and cannot be edited from this feature.

### Goals

- Allow authenticated users to view their profile information.
- Allow users to update personal profile fields safely.
- Keep email read-only because it identifies the user account.
- Reduce support tickets for basic profile changes.

### Out-of-scope

- Email change flow.
- Password change and password recovery.
- Company information management.
- Business Unit membership management.
- Address management.
- Avatar / profile photo upload.
- Notification preferences.

### Measurable Outcomes

- Profile view rate.
- Profile edit success rate.
- Profile validation failure rate.
- Reduction in profile-related support tickets.

## Requirements

### 10.1 User views profile information

- [P0] User must be authenticated to access Profile Management.
- [P0] System shows the user's current profile information.
- [P0] System shows Email as a read-only field.
- [P0] System shows Fullname, Gender, Phone, and Birthday as editable profile fields.
- [P0] If Fullname is missing, system defaults Fullname from the user's Email by extracting the email local-part before `@`.
- [P0] System must not allow user to edit Email from Profile Management.
- [P0] If profile data cannot be loaded, system shows a general error and allows user to retry.

### 10.2 User edits editable profile fields

- [P0] User can edit Fullname.
- [P0] User can edit Gender.
- [P0] User can edit Phone.
- [P0] User can edit Birthday.
- [P0] User can submit profile changes.
- [P0] System validates the submitted profile fields before saving.
- [P0] If validation succeeds, system saves the updated profile information.
- [P0] After saving succeeds, system shows a success message and displays the latest profile values.
- [P0] If saving fails, system shows a general error and keeps the user's entered values on the screen.

### 10.3 Email is read-only identity field

- [P0] System uses Email as a user identity field.
- [P0] Email must be displayed in Profile Management for user reference.
- [P0] Email must be read-only and disabled from direct editing.
- [P0] Profile update submission must not include Email as an editable value.
- [P0] System must reject any profile update request that attempts to change Email through this feature.

### 10.4 Profile field validation

- [P0] Fullname is required.
- [P0] Fullname must not be empty after trimming whitespace.
- [P0] If user clears Fullname and saves, system must reject the submission instead of re-extracting Fullname from Email.
- [P0] Phone is required.
- [P0] Phone must use a valid phone-number format accepted by MDFoods.
- [P0] Gender is optional unless required by downstream business rules.
- [P0] If Gender is provided, it must match one of the supported profile gender options.
- [P0] Birthday is optional unless required by downstream business rules.
- [P0] If Birthday is provided, it must be a valid date.
- [P0] Birthday must not be a future date.
- [P0] If any validation fails, system shows field-level validation messages and prevents saving.

### 10.5 User cancels profile edits

- [P1] User can cancel profile editing before saving.
- [P1] If user cancels editing, system discards unsaved changes and restores the last saved profile values.
- [P1] If user navigates away with unsaved changes, system should warn user before discarding changes.

## Appendix

### Editable Fields

| Field | Editable | Required | Notes |
|---|---|---|---|
| Email | No | Yes | Read-only identity field; cannot be changed from Profile Management |
| Fullname | Yes | Yes | User's display/legal contact name |
| Gender | Yes | No | Optional profile attribute unless required by business rules |
| Phone | Yes | Yes | User contact phone number |
| Birthday | Yes | No | Optional date; cannot be future date |

### Email Identity Rule

- Email identifies the user account in MDFoods.
- Profile Management can display Email but must not provide direct Email editing.
- Any future Email change flow must be handled as a separate verified flow and is outside this feature.

### Default Profile Value Rule

- If a user account does not have Fullname yet, system derives the initial Fullname from Email.
- The default Fullname is the email local-part before `@`.
- Example: `chef@restaurant.com` defaults Fullname to `chef`.
- This default extraction only applies when displaying or initializing a missing Fullname. It must not bypass validation after the user edits the field.
