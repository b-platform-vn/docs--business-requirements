# Software Development Life Cycle (SDLC)

> **First version — 2026-08-11.**
> The standard process by which the B-Platform Software Development Team turns a **Business request** into a **Running feature in Production**.
> This document is the canonical SDLC for the `github.com/bplatform-vn` organization. It binds **people, roles, repos, and tooling** to clear **Who / What / When** ownership.

---

## 1. Purpose & scope

### 1.1 What is an SDLC?

The **Software Development Life Cycle (SDLC)** is the structured process a Software Development Team follows to take a feature from a business idea all the way to running software in production, and to keep it running. A mature SDLC answers three questions for every piece of work:

- **Who** is accountable and who is responsible at each step?
- **What** artifacts, changes, and verifications must exist before moving on?
- **When** does each step start and end, and what gate must be passed to proceed?

SDLC is tool which help managing tasks is easier, more transparent, more effective. Product companies that scale (Google, Spotify, Shopify, Atlassian, etc.) all converge on the same canonical phases; what differs is the tooling, the team topology, and the rigor of the gates. This document adopts that canonical shape and adapts it to B-Platform's architecture and its **human development team**.

### 1.2 Scope

| In scope                                                 | Out of scope                                                           |
| -------------------------------------------------------- | ---------------------------------------------------------------------- |
| Business request intake → production release → operation | Sales, commercial negotiation, marketing launch                        |
| All `bplatform-vn` repos (L0–L3 + platform infra)        | Third-party SaaS administration (except where it gates a release)      |
| Human roles and team collaboration                       | HR/people management                                                   |
| Code, design, tests, infra, and docs artifacts           | Customer support ticket handling (covered by Operations feedback only) |

---

## 2. SDLC at a glance

```mermaid
flowchart LR
  BR[Business Request] --> P1[1. Discovery & Triage]
  P1 --> P2[2. Analysis & Design]
  P2 --> P3[3. Implementation]
  P3 --> P4[4. Testing & Quality]
  P4 --> P5[5. Release & Deployment]
  P5 --> P6[6. Operations & Feedback]
  P6 -.feedback.-> P1
  P6 --> DONE[Running feature in Production]

  style BR fill:#1a73e8,color:#fff
  style DONE fill:#34a853,color:#fff
  style P1 fill:#E91E63,color:#fff
  style P2 fill:#9C27B0,color:#fff
  style P3 fill:#FF9800,color:#fff
  style P4 fill:#FF5722,color:#fff
  style P5 fill:#607D8B,color:#fff
  style P6 fill:#455A64,color:#fff
```

| #   | Phase                 | Trigger                                                       | Exit gate (Definition of Done)                              |
| --- | --------------------- | ------------------------------------------------------------- | ----------------------------------------------------------- |
| 1   | Discovery & Triage    | A business need, bug, or idea is raised                       | Triaged Multica issue with type, priority, and scope exists |
| 2   | Analysis & Design     | Triaged issue moved to **Todo**                               | Approved FRD + Architecture decision + Figma design linked  |
| 3   | Implementation        | Issue moved to **In Progress**, assigned to Software Engineer | Code merged to `main` via passing PR                        |
| 4   | Testing & Quality     | PR opened                                                     | All automated gates green on `main`                         |
| 5   | Release & Deployment  | Merge to `main` passes gates                                  | Feature reachable in **staging**, then **production**       |
| 6   | Operations & Feedback | Feature live in production                                    | Monitored, documented, and feeding back into Discovery      |

---

## 3. Standard SDLC phases (industry baseline)

This section describes the canonical SDLC as applied in product companies, so the B-Platform adaptation in §4 is grounded in a shared vocabulary.

1. **Planning & Requirement Analysis** — Stakeholders define _what_ the business needs and _why_. Output: a prioritized backlog of spec'd requests (a Product Requirement Document, PRD, or a lighter Feature Requirement Document, FRD).
2. **System Design** — Architects and designers define _how_ the system will meet the requirements: architecture decisions (ADRs), API contracts, data models, and UI/UX design. Output: design artifacts, ADRs, API contracts.
3. **Implementation / Development** — Engineers write code that realizes the design across the agreed layers, following branching and code-review conventions. Output: reviewed and merged pull requests.
4. **Testing** — Automated and manual verification that the change behaves as specified and does not regress existing behavior: unit, integration, E2E, security, and performance checks. Output: green CI, test reports.
5. **Deployment** — The verified change is promoted through environments (dev → staging → production) using an automated pipeline with explicit approval gates. Output: a live, tagged release.
6. **Maintenance & Operations** — The released feature is monitored, supported, and improved based on real-world feedback and incidents. Output: runbooks, metrics, and new feedback items re-entering phase 1.

Modern product companies compress these into a **continuous flow** (CI/CD, trunk-based development, feature flags, progressive delivery), but the phase boundaries and their gates remain — they are what make the flow safe.

---

## 4. B-Platform SDLC — adapted

The B-Platform SDLC maps the canonical phases onto the Super App kernel architecture and onto the **human development team** — the people who collaborate to deliver software.

### 4.1 Roles

| Role                     | Owns                                                                                           | Person in Charge (PiC)                                                                         |
| ------------------------ | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **Business Stakeholder** | Originates business needs, validates outcomes, accepts features                                | Thái, Nhi                                                                                      |
| **Product Owner**        | Prioritizes backlog, accepts specs, signs release scope                                        | Long                                                                                           |
| **Business Analyst**     | Product specs, acceptance criteria, FRDs, issue triage                                         | Long (An supports)                                                                             |
| **Solution Architect**   | ADRs, repo topology, SOLID boundaries, work-breakdown plans                                    | Long                                                                                           |
| **UI/UX Designer**       | Figma designs, components, mockups, design-to-code sync                                       | _TBD_                                                                                          |
| **Software Engineer**    | Feature branches, code, tests, repo-level docs, PRs across L0–L3                               | An; Phát Phan 1 (Freelance); Phát Phan 2 (Freelance); Phát Ngô (Freelance); Khương (Freelance) |
| **DevOps Engineer**      | `platform-fluxcd`, `platform-workflows`, k8s clusters (stg/prd), secrets, CI/CD, PR merge gate | Long                                                                                           |
| **QA Engineer**          | Test planning, manual/exploratory testing, quality sign-off                                    | Software Engineers cross-check                                                                 |

> **Source of truth rule (non-negotiable):** the remote `main` branch of `github.com/bplatform-vn/*` is the single source of truth (SSoT). Local clones are mirrors only. If local and remote disagree, remote wins.

### 4.2 Tooling & where artifacts live

| Concern                | Tool / Location                                                | Notes                                                                                                              |
| ---------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Work tracking          | **Multica** (self-hosted)                                      | Issues, tasks, projects, status workflow                                                                           |
| Communication          | **Rocket.Chat** (self-hosted - `https://rocket.b-platform.vn`) | Team chat; business requests land in `#business-requests` (`https://rocket.b-platform.vn/group/business-requests`) |
| Source control & PRs   | **GitHub** (Free plan - `github.com/bplatform-vn`)             | Default branch `main`; PRs required                                                                                |
| Design source of truth | **Figma** (Paid plan)                                          | B-Platform Super App file + per-product files                                                                      |
| CI/CD pipelines        | **`platform-workflows`** (GitHub Actions)                      | Build, test, security scans, publish                                                                               |
| GitOps deployment      | **`platform-fluxcd`**                                          | Staging = `k8s-dpsrv`, Production = `k8s-dpsrv-prd`                                                                |

---

## 5. Phases in detail — Who does What at When

Each phase below is told as a **human collaboration story**: who talks to whom, where (Rocket.Chat channel / Multica issue), what each person contributes, and the handoff that closes the phase. Pipeline mechanics (`platform-workflows`, `platform-fluxcd`, CI scans) stay in the background as the tooling that supports the handoffs — the focus is the people.

A shared **release thread** (a Rocket.Chat channel or a pinned Multica issue comment) follows the request across all phases so any teammate can open one place and see the whole conversation.

### Phase 1 — Discovery & Triage

**When it starts:** A Business Stakeholder posts a need in **`#business-requests`** (`https://rocket.b-platform.vn/group/business-requests`) — a new feature, a bug, or a change request, written in plain business language.

**The collaboration:**

1. The **Business Analyst** acknowledges the post in the same channel, asks clarifying questions, and confirms understanding back to the Business Stakeholder before writing anything down.
2. The Business Analyst captures the need as a **Multica issue** (short description under ~600 chars — link to the source of truth, don't paste the whole FRD;) and posts the issue link back into the `#business-requests` thread so the Business Stakeholder can follow it.
3. The Business Analyst classifies the issue (type / priority / product / tentative affected layers), checks for duplicates, links related issues, and places it in **Backlog**.
4. The **Product Owner** reviews the triaged Backlog and decides what to invest in this cycle.

**Who does what:**

| Activity                               | Owner                | Support              |
| -------------------------------------- | -------------------- | -------------------- |
| Raise the need in `#business-requests` | Business Stakeholder | Product Owner        |
| Acknowledge & clarify in-channel       | Business Analyst     | Business Stakeholder |
| Create Multica issue + post link back  | Business Analyst     | Product Owner        |
| Classify, dedupe, link                 | Business Analyst     | Product Owner        |
| Review Backlog & decide investment     | Product Owner        | Business Analyst     |

**Exit gate:** A triaged Multica issue exists with **type**, **priority**, **product**, and a one-sentence scope; the Business Stakeholder has the issue link and can follow progress. The issue is ready to be pulled into a cycle.

---

### Phase 2 — Analysis & Design

**When it starts:** The Product Owner moves the triaged issue from **Backlog** to **Todo** and announces in the release thread that the request is being specced.

**The collaboration:**

1. The **Business Analyst** drafts the **Feature Requirement Document (FRD)** with acceptance criteria, consulting the Business Stakeholder in `#business-requests` (or a dedicated thread) for any unknowns. The FRD lives under `products/<product>/features/`; only scope + links go into Multica.
2. The **Solution Architect** reviews the FRD and decides the technical approach with the assigned **Software Engineer** in the release thread: which layers are touched, whether a new repo is needed (per the v3 naming convention), and records an **ADR** for non-trivial decisions. For large efforts, the Architect posts a **Work-Breakdown Structure** with per-task assignment guidance (template: `technical-requirements/dbo-implementation-plan.md`).
3. The **UI/UX Designer** produces or updates the **Figma design** (Web / iPad / Mobile viewports per the B-Platform design catalog rules), walking the Product Owner through it for sign-off, and links the Figma URL in the FRD.
4. If a new repo is required, the **DevOps Engineer** creates it under `github.com/bplatform-vn` (per the v3 naming convention, seeded with `README.md` + `.gitignore` + initial commit on `main`) and posts the repo link in the release thread.
5. The **Product Owner** walks the **Business Stakeholder** through the final FRD + design in `#business-requests` for sign-off.

**Who does what:**

| Activity                                 | Owner              | Support                               |
| ---------------------------------------- | ------------------ | ------------------------------------- |
| Draft FRD + acceptance criteria          | Business Analyst   | Business Stakeholder (clarifications) |
| Decide technical approach + ADR          | Solution Architect | Software Engineer, DevOps Engineer    |
| Post work-breakdown plan (large efforts) | Solution Architect | Software Engineer                     |
| Produce Figma design + walk through      | UI/UX Designer     | Product Owner                         |
| Create new repo (if needed)              | DevOps Engineer    | Solution Architect                    |
| Present FRD + design to business         | Product Owner      | Business Stakeholder                  |
| Accept the spec                          | Product Owner      | Business Stakeholder                  |

**Exit gate:** FRD is **approved** by the Product Owner and walked through with the Business Stakeholder; an ADR exists for any non-trivial decision; the Figma design is linked (or `TBD` with a due date); affected repos are known. The issue is ready to be assigned to a Software Engineer and moved to **In Progress**.

---

### Phase 3 — Implementation

**When it starts:** The Product Owner assigns the issue to a **Software Engineer** and moves it to **In Progress**. The Product Owner posts the assignment in the release thread so the team knows who owns it.

**The collaboration:**

1. The Software Engineer branches from a **fresh remote `main`** (never a stale local clone — SSoT rule). Branch naming: `<issue-key>-<short-slug>` (e.g. `BPL-123-order-total-calc`). They post the branch name in the release thread.
2. During implementation, the Software Engineer consults the **Solution Architect** in the release thread whenever a contract or boundary question comes up — design decisions are recorded, not left in DMs.
3. The Software Engineer writes unit tests alongside the code (vitest for NestJS packages — see `/memories/repo/nestjs-vitest-decorator-metadata.md` for the required `unplugin-swc` setup) and updates repo-level docs (`README.md`, JSDoc/TSDoc) in the same repo.
4. When ready, the Software Engineer opens a **Pull Request** against `main`, references the Multica issue key in the PR title/body, and requests review from the **DevOps Engineer** (PR gate). They post the PR link in the release thread.
5. The DevOps Engineer reviews the PR; the Software Engineer addresses review comments in-thread. The Software Engineer who built it stays available to answer the reviewer's questions.

**Who does what:**

| Activity                                | Owner             | Support                               |
| --------------------------------------- | ----------------- | ------------------------------------- |
| Announce assignment in release thread   | Product Owner     | —                                     |
| Branch + post branch name               | Software Engineer | —                                     |
| Consult Architect on contract questions | Software Engineer | Solution Architect                    |
| Write code + unit tests + repo docs     | Software Engineer | Solution Architect                    |
| Open PR + post PR link                  | Software Engineer | —                                     |
| Review PR (gate) + merge decision       | DevOps Engineer   | Software Engineer, Solution Architect |

**Exit gate:** PR is **approved** and **merged to `main`** by the DevOps Engineer. The merge commit SHA (posted in the release thread) is the canonical reference for the change.

---

### Phase 4 — Testing & Quality

**When it starts:** The PR is opened — testing is continuous, not a separate waterfall stage. The release thread is where test status and findings are discussed.

**The collaboration:**

1. `platform-workflows` runs CI on the PR (build, lint, unit tests, integration/E2E where present) and security scans. The DevOps Engineer owns the pipeline and surfaces failures in the release thread.
2. The **Software Engineer** triages and fixes CI/security findings in-thread; the DevOps Engineer confirms re-runs are green.
3. The **QA Engineer** performs manual/exploratory testing on the PR branch (and on staging for production-impacting changes), posting a test report to the release thread: what was checked, what passed, what failed.
4. On merge to `main`, the full CI suite runs again; the DevOps Engineer confirms the `main` build is green in-thread.
5. Any defects found by QA are posted back to the Software Engineer. **Blockers** return the work to Phase 3 (the Software Engineer reopens the branch); **non-blockers** are filed as new Multica issues for a later cycle.
6. The **QA Engineer** gives the final quality sign-off in the release thread for production-impacting changes.

**Who does what:**

| Activity                                 | Owner             | Support                        |
| ---------------------------------------- | ----------------- | ------------------------------ |
| Run CI + security scans, surface results | DevOps Engineer   | platform-workflows             |
| Triage & fix CI/security findings        | Software Engineer | DevOps Engineer                |
| Manual/exploratory testing + report      | QA Engineer       | Software Engineer              |
| Re-run CI on `main` + confirm green      | DevOps Engineer   | —                              |
| Post defects; blockers → Phase 3         | QA Engineer       | Software Engineer              |
| Final quality sign-off                   | QA Engineer       | DevOps Engineer, Product Owner |

**Exit gate:** All CI checks on `main` are **green**, the security scan is clean, the QA Engineer has signed off in the release thread, and (for production-impacting changes) the Product Owner has acknowledged the sign-off. The change is eligible for release.

---

### Phase 5 — Release & Deployment

**When it starts:** Merge to `main` passes all gates in Phase 4.

**The collaboration:**

1. The **DevOps Engineer** deploys the build to **staging** (`k8s-dpsrv`) and posts a **staging-ready notice** in the release thread, tagging the Product Owner and (for business-visible features) the Business Stakeholder, with: the Multica issue key, what changed, how to verify, and the staging URL.
2. **Staging verification (human collaboration):**
   - The **Product Owner** runs through the FRD acceptance criteria on staging and posts findings.
   - The **Business Stakeholder** (optional) validates business-visible outcomes against the original request and posts confirmation or concerns.
   - The **Software Engineer** who built the feature is on standby to answer questions or apply quick fixes if staging surfaces issues.
   - Findings are posted back to the release thread and the Multica issue. **Blockers** return the work to Phase 3; **non-blockers** are filed as new issues for a later cycle and do not block this release.
3. **Production go/no-go:** once staging is signed off, the **Product Owner** posts an explicit **"approved for production"** decision in the release thread. For hotfixes, the DevOps Engineer may give this approval with the Product Owner informed. Production deploys are **never** automatic from a single PR merge — the human approval in the release thread is the gate.
4. The **DevOps Engineer** promotes the same image tag to **production** (`k8s-dpsrv-prd`), announces the production deploy in the release thread (with the tag, timing, and a rollback contact), and **tags the release in Git** (`v<version>` or `<repo>-<sha>`) — the tag is the rollback unit.
5. After deploy, the DevOps Engineer confirms production health in the release thread. The **Product Owner** marks the Multica issue **Done** and links the release tag.

**Who does what:**

| Activity                                      | Owner                                        | Support           |
| --------------------------------------------- | -------------------------------------------- | ----------------- |
| Deploy to staging + post staging-ready notice | DevOps Engineer                              | platform-fluxcd   |
| Verify against FRD on staging                 | Product Owner                                | Software Engineer |
| Validate business outcomes on staging         | Business Stakeholder                         | Product Owner     |
| Standby for staging fixes                     | Software Engineer                            | —                 |
| Report findings / blockers in release thread  | Product Owner / Business Stakeholder         | Software Engineer |
| Approve production deploy (go/no-go)          | Product Owner (DevOps Engineer for hotfixes) | —                 |
| Promote to production + tag release           | DevOps Engineer                              | platform-fluxcd   |
| Announce production deploy + confirm health   | DevOps Engineer                              | —                 |
| Mark Multica issue Done + link tag            | Product Owner                                | DevOps Engineer   |
| Rollback if needed                            | DevOps Engineer                              | Product Owner     |

**Exit gate:** The feature is **reachable in production**, the release is tagged and announced in the release thread, and the Multica issue is **Done**. The feature is now a "Running feature in Production."

---

### Phase 6 — Operations & Feedback

**When it starts:** The feature is live in production and the DevOps Engineer has confirmed health in the release thread.

**The collaboration:**

1. The **DevOps Engineer** watches production health (cluster, error rates, DBO worker heartbeats, orchestrator DLQ, sync-audit) and confirms runbooks/observability dashboards are up to date. Anything abnormal is posted to the release thread and the on-call channel.
2. If an incident or regression is found, the **DevOps Engineer** and **Product Owner** decide severity together in chat. Incidents are filed as **new Multica issues** (type: bug) and re-enter Phase 1 — they are _not_ patched in place. The **Software Engineer** is pulled in for a hotfix (shortened Phase 3–5) when the incident is code-related.
3. The **Business Stakeholder** observes the live feature and shares usage feedback / new ideas back in `#business-requests`, which the Business Analyst captures as new issues for the next cycle.
4. The **Solution Architect** syncs `code_bases/<repo>.md` to reflect the new public API/behavior (diff before write — only changed sections) and closes the loop on the release thread.
5. The **Product Owner** formally accepts the long-term outcome with the Business Stakeholder and closes the release thread.

**Who does what:**

| Activity                                  | Owner                           | Support              |
| ----------------------------------------- | ------------------------------- | -------------------- |
| Watch production health + update runbooks | DevOps Engineer                 | —                    |
| Triage incident severity                  | DevOps Engineer + Product Owner | Software Engineer    |
| File incident as new Multica issue        | Product Owner / DevOps Engineer | Business Stakeholder |
| Hotfix (code-related incidents)           | Software Engineer               | DevOps Engineer      |
| Share usage feedback / new ideas          | Business Stakeholder            | Product Owner        |
| Capture feedback as new issues            | Business Analyst                | Business Stakeholder |
| Sync `code_bases/` docs                   | Solution Architect              | —                    |
| Accept long-term outcome + close thread   | Product Owner                   | Business Stakeholder |

**Exit gate:** The feature is monitored, documented, and any follow-up work is captured as new triaged issues in `#business-requests` / Multica. The release thread is closed — and the cycle reopens via Phase 1 for the next request.

---

## 6. Master matrix — Who does What at When

This is the one-table summary of the whole SDLC. **A** = Accountable (final sign-off), **R** = Responsible (does the work), **C** = Consulted, **I** = Informed.

| #   | Activity                        | Phase | Business Stakeholder | Product Owner | Business Analyst | Solution Architect | UI/UX Designer | Software Engineer | DevOps Engineer | QA Engineer |
| --- | ------------------------------- | ----- | -------------------- | ------------- | ---------------- | ------------------ | -------------- | ----------------- | --------------- | ----------- |
| 1   | Raise business need             | 1     | **R**                | C             | I                | I                  | I              | –                 | –               | –           |
| 2   | Create & classify Multica issue | 1     | I                    | **A**         | **R**            | C                  | –              | –                 | –               | –           |
| 3   | Write FRD + acceptance criteria | 2     | C                    | **A**         | **R**            | C                  | C              | –                 | –               | –           |
| 4   | Architecture decision / ADR     | 2     | –                    | I             | C                | **A/R**            | –              | C                 | C               | –           |
| 5   | Work-breakdown plan             | 2     | –                    | I             | –                | **A/R**            | –              | C                 | C               | –           |
| 6   | Figma design                    | 2     | C                    | **A**         | C                | I                  | **R**          | –                 | –               | –           |
| 7   | Create new repo (if needed)     | 2     | –                    | I             | –                | C                  | –              | –                 | **A/R**         | –           |
| 8   | Branch & implement              | 3     | –                    | I             | –                | I                  | C              | **A/R**           | –               | –           |
| 9   | Write unit tests                | 3     | –                    | –             | –                | –                  | –              | **A/R**           | –               | –           |
| 10  | Open PR                         | 3     | –                    | I             | –                | C                  | –              | **R**             | **A**           | –           |
| 11  | Run CI on PR & `main`           | 4     | –                    | –             | –                | –                  | –              | –                 | **A/R**         | –           |
| 12  | Triage & fix CI failures        | 4     | –                    | –             | –                | C                  | –              | **R**             | **A**           | –           |
| 13  | Security scan sign-off          | 4     | –                    | I             | –                | –                  | –              | –                 | **A/R**         | C           |
| 14  | Manual/exploratory testing      | 4     | –                    | –             | –                | –                  | –              | I                 | I               | **A/R**     |
| 15  | Final quality gate              | 4     | –                    | **A**         | –                | –                  | –              | I                 | R               | **R**       |
| 16  | Publish artifacts               | 5     | –                    | I             | –                | –                  | –              | –                 | **A/R**         | –           |
| 17  | Deploy to staging               | 5     | –                    | C             | –                | –                  | –              | –                 | **A/R**         | –           |
| 18  | Verify in staging               | 5     | C                    | **A**         | –                | –                  | –              | C                 | I               | C           |
| 19  | Approve & deploy to production  | 5     | I                    | **A**         | –                | –                  | –              | –                 | **R**           | –           |
| 20  | Tag the release                 | 5     | –                    | I             | –                | –                  | –              | –                 | **A/R**         | –           |
| 21  | Rollback (if needed)            | 5     | –                    | C             | –                | –                  | –              | –                 | **A/R**         | –           |
| 22  | Monitor production              | 6     | I                    | I             | –                | –                  | –              | –                 | **A/R**         | –           |
| 23  | Incident → new Multica issue    | 6     | I                    | **A**         | **R**            | –                  | –              | –                 | R               | –           |
| 24  | Sync `code_bases/` docs         | 6     | –                    | –             | –                | **A/R**            | –              | –                 | –               | –           |
| 25  | Accept long-term outcome        | 6     | C                    | **A**         | –                | –                  | –              | –                 | –               | –           |

---

## 7. Multica issue status workflow

The Multica issue status mirrors the SDLC phases. Mapping issue status → SDLC phase → who is active:

| Multica issue status                            | SDLC phase             | Who is active                                              |
| ----------------------------------------------- | ---------------------- | ---------------------------------------------------------- |
| **Backlog**                                     | (pre-Phase 1)          | Business Analyst classifies                                |
| **Todo**                                        | Phase 2 starts         | Business Analyst, Solution Architect, UI/UX Designer       |
| **In Progress** (assigned to Software Engineer) | Phase 3 starts         | Software Engineer                                          |
| **In Review** (PR open)                         | Phase 3 → Phase 4      | Software Engineer, DevOps Engineer (PR gate), QA Engineer  |
| **In Staging**                                  | Phase 5 (staging)      | DevOps Engineer, Product Owner, Business Stakeholder       |
| **Done**                                        | Phase 5 exit / Phase 6 | Feature is live; DevOps Engineer + Product Owner take over |

> **Workflow note:** issues are pulled into a cycle by moving them from **Backlog** → **Todo** → **In Progress**. Assignment to a Software Engineer happens at the **In Progress** transition. Reassigning or unassigning an in-progress issue returns it to **Todo** until it is re-picked up.

---

## 8. Artifacts & source of truth per phase

| Phase | Artifact                            | Source of truth                              | Owner              |
| ----- | ----------------------------------- | -------------------------------------------- | ------------------ |
| 1     | Multica issue (type/priority/scope) | Multica                                      | Business Analyst   |
| 2     | FRD + acceptance criteria           | `products/<product>/features/*.md`           | Business Analyst   |
| 2     | Architecture Decision Record        | `/memories/repo/*.md` + ADR docs             | Solution Architect |
| 2     | Work-breakdown plan                 | `technical-requirements/*.md`                | Solution Architect |
| 2     | Figma design                        | Figma file URL                               | UI/UX Designer     |
| 2     | New repo (if any)                   | `github.com/bplatform-vn/<repo>` `main`      | DevOps Engineer    |
| 3     | Feature branch + PR                 | GitHub PR                                    | Software Engineer  |
| 3     | Code + tests + repo docs            | `github.com/bplatform-vn/<repo>` `main`      | Software Engineer  |
| 4     | CI run results                      | `platform-workflows` runs                    | DevOps Engineer    |
| 4     | Test plan & exploratory test notes  | QA workspace (Multica issue comments / wiki) | QA Engineer        |
| 5     | Published image/package             | Container registry / npm `@b-platform-vn/*`  | DevOps Engineer    |
| 5     | Release tag                         | Git tag on `main`                            | DevOps Engineer    |
| 5     | Staging deployment                  | `platform-fluxcd` + `k8s-dpsrv`              | DevOps Engineer    |
| 5     | Production deployment               | `platform-fluxcd` + `k8s-dpsrv-prd`          | DevOps Engineer    |
| 6     | Synced `code_bases/<repo>.md`       | `platform-ecosystem-docs/code_bases/`        | Solution Architect |

---

## 9. Definition of Done (per phase) — quick checklist

- **Phase 1:** ✅ Multica issue exists with type, priority, product, one-sentence scope.
- **Phase 2:** ✅ FRD approved; ✅ ADR recorded (if non-trivial); ✅ Figma linked or TBD-with-date; ✅ affected repos identified.
- **Phase 3:** ✅ PR opened against fresh `main`; ✅ Super App kernel layer boundaries respected; ✅ unit tests authored; ✅ repo docs updated.
- **Phase 4:** ✅ CI green on PR and on `main`; ✅ security scan clean; ✅ QA Engineer sign-off for production-impacting changes.
- **Phase 5:** ✅ Artifacts published; ✅ staging verified & signed off; ✅ production deployed & tagged; ✅ Multica issue → Done.
- **Phase 6:** ✅ Monitoring confirmed; ✅ runbooks/dashboards updated; ✅ follow-ups filed as new issues; ✅ `code_bases/` synced.

---

## 10. What's next (v2 candidates)

This is **version 1** — focused on establishing the Who/What/When baseline. Candidate improvements for v2:

- **Feature flags & progressive delivery** — decouple merge from release.
- **Formal SLOs and error budgets** — tie release cadence to reliability.
- **Hotfix fast-path** — a shortened Phase 3–5 flow for P0 production incidents.
- **Cross-repo release coordination** — when one feature touches L0 + L1 + L2 + L3 simultaneously.
- **Security & compliance gates** — formal threat-model and compliance checklist per release.
- **Metrics & retrospectives** — measure cycle time, lead time, MTTR, and feed back quarterly.

> Change log: **v1 (2026-08-11)** — initial SDLC: 6 phases, human-role RACI, master matrix, Multica issue status workflow, artifacts, and Definition of Done.

> 🇬🇧 **English** · [🇻🇳 Tiếng Việt](sdlc/README.vi.md)
