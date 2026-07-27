# Legal Pages — Term of Use & Privacy Policy

| | |
|---|---|
| **Author** | tvlong |
| **Updated** | 2026.07.27 |
| **Product** | [LFarm](/products/lfarm/README.md) |
| **Priority** | P1 |
| **Tracklogs** | [Multica BPL-282](https://tasks.tvlong.me/issues/BPL-282) |

---

## The Problem

LFarm currently shows footer links for **Term of Use** and **Privacy Policy**, but the pages are missing or not wired. Clicking those footer links does not navigate users anywhere.

- Users expect legal and privacy links in the footer to open readable pages.
- Non-functional footer legal links reduce trust in the public website.
- Missing terms and privacy information may create compliance risk.
- Vietnamese and English users both need access to locale-appropriate legal information.

## Proposed Solution

Create working **Term of Use** and **Privacy Policy** pages for LFarm and wire the existing footer links to those pages in both Vietnamese and English locales.

The pages should use approved legal/privacy content when available. If final content is not yet approved, pages may launch with clearly labeled placeholder content approved by Product/Legal so users never encounter dead footer links.

### Goals

- Provide working legal-information pages for LFarm.
- Ensure footer legal links navigate correctly instead of doing nothing.
- Support Vietnamese and English locales.
- Improve public-site trust and compliance readiness.
- Allow users and stakeholders to share direct links to legal pages.

### Out-of-scope

- Drafting final legal copy inside engineering scope.
- Building a CMS/legal-document management workflow.
- Changing unrelated footer links or landing-page content.
- Adding account-specific consent tracking.

### Measurable Outcomes

- Footer legal-link click-throughs navigate successfully with no dead clicks.
- Zero broken-link reports for Term of Use and Privacy Policy footer links.
- Legal page views are trackable after release.

## Requirements

### 1. Term of Use Page

- [P0] System provides a Term of Use page for LFarm.
- [P0] Vietnamese users can open the Vietnamese Term of Use page.
- [P0] English users can open the English Term of Use page.
- [P0] The page has a localized page title, main heading, and body content.
- [P1] The page has SEO-friendly metadata.
- [P1] The page supports direct URL access and browser refresh.

### 2. Privacy Policy Page

- [P0] System provides a Privacy Policy page for LFarm.
- [P0] Vietnamese users can open the Vietnamese Privacy Policy page.
- [P0] English users can open the English Privacy Policy page.
- [P0] The page has a localized page title, main heading, and body content.
- [P1] The page has SEO-friendly metadata.
- [P1] The page supports direct URL access and browser refresh.

### 3. Footer Link Behavior

- [P0] When a Vietnamese user clicks the footer Term of Use link, the system navigates to the Vietnamese Term of Use page.
- [P0] When an English user clicks the footer Term of Use link, the system navigates to the English Term of Use page.
- [P0] When a Vietnamese user clicks the footer Privacy Policy link, the system navigates to the Vietnamese Privacy Policy page.
- [P0] When an English user clicks the footer Privacy Policy link, the system navigates to the English Privacy Policy page.
- [P0] Legal page links preserve the user's current locale.
- [P0] Clicking either footer legal link must not result in no action.
- [P1] Browser back navigation returns users to the previous page after visiting a legal page.

### 4. Content Readiness

- [P0] Product/Legal/business owner provides or approves Term of Use content before final release.
- [P0] Product/Legal/business owner provides or approves Privacy Policy content before final release.
- [P1] If final content is not ready, pages display clearly labeled placeholder content approved for temporary release.

## User Flow / UX Notes

1. User opens the LFarm landing page.
2. User scrolls to the footer.
3. User clicks **Term of Use** or **Privacy Policy**.
4. System navigates to the corresponding legal page in the current locale.
5. User reads the page content.
6. User uses browser back navigation to return to the previous page when needed.

## Edge Cases

- If a user opens a legal page URL directly, the page renders successfully.
- If a user refreshes a legal page, the page renders successfully.
- If the locale is unknown or unsupported, the page follows existing LFarm locale fallback behavior.
- If approved final content is unavailable, the page displays approved placeholder content instead of a blank page or broken route.

## Dependencies

- Product/Legal/business owner must provide or approve final Term of Use content.
- Product/Legal/business owner must provide or approve final Privacy Policy content.
- Engineering must confirm final route paths for both locales.
- Footer implementation must expose clickable links for both pages.

## Acceptance Criteria

- `Term of Use` footer link opens a working Terms page on the Vietnamese site.
- `Term of Use` footer link opens a working Terms page on the English site.
- `Privacy Policy` footer link opens a working Privacy Policy page on the Vietnamese site.
- `Privacy Policy` footer link opens a working Privacy Policy page on the English site.
- Clicking either footer link no longer results in no action.
- Direct navigation to each legal page URL renders successfully.
- Page title, main heading, and locale-specific content are present for both pages.
- Browser back navigation works after visiting each legal page.
- QA verifies the links on desktop and mobile views for both Vietnamese and English locales.

## Risks

- Missing or unapproved legal content may delay release.
- Incorrect privacy wording may create compliance or customer-trust risk.
- Inconsistent locale routing may cause users to land on the wrong language version.

## Appendix

### Related Multica Issue

- [BPL-282 — [LFarm] Add Term of Use and Privacy Policy pages](https://tasks.tvlong.me/issues/BPL-282)
