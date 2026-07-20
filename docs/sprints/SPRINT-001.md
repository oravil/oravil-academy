# Sprint 1

**Sprint Number:** 001
**Status:** In Progress
**Date:** 2026-07-16
**Reference:** OA-MVP-008 MVP Backend Architecture, OA-MVP-009 MVP Frontend Architecture, OA-MVP-010 MVP Implementation Plan, OA-GUIDE-001 Project Execution Guide

---

## Sprint Goal

Deliver a working, authenticated learner session and a fully functional Module Overview screen. At the end of Sprint 1, a learner can authenticate, establish a valid session, and view the Module Overview showing the four lessons of Module 1 with correct initial status — Lesson 1 available, Lessons 2 through 4 locked — backed by real data from the database and confirmed progress state from the API.

---

## Scope

This sprint covers two items from the Implementation Plan:

- Step 1 — Project Foundation (database, authentication infrastructure, health check)
- Step 2 — Content Seeding (Phase 0, Module 1 content in the database)
- Step 3 — Authentication (Sanctum SPA session establishment and verification per ADR-0006)
- Step 4 — Module Overview (API endpoints and screen)

Step 5 and beyond are explicitly out of scope for this sprint.

---

## User Stories

---

### Story 1 — Learner Authentication

**Story:**
As a pilot learner, I want to receive access credentials and authenticate so that the system recognises me and I can begin the module.

**Business Value:**
Authentication is the prerequisite for all learner-specific behaviour — progress tracking, submission gating, and survey completion. Without it, no personalised learning experience is possible.

**Acceptance Criteria:**

1. A learner record can be created in the database with a unique email address.
2. The learner can authenticate using their credentials and establish an authenticated session.
3. A request to any protected endpoint with no authenticated session receives a 401 response.
4. A request to any protected endpoint with an invalid or expired session receives a 401 response.
5. A valid session is accepted by the API and the authenticated learner identity is resolved correctly.

---

### Story 2 — Module Overview

**Story:**
As an authenticated learner, I want to see the Module Overview so that I understand what Module 1 contains, what the module deliverable is, and which lesson I should begin.

**Business Value:**
The Module Overview is the learner's entry point into the learning experience. It sets expectations for what the module covers and provides a clear, single call to action. A correctly functioning Module Overview with accurate progress state is the foundation on which every subsequent feature depends.

**Acceptance Criteria:**

1. `GET /v1/modules/{module_id}/overview` returns the module title, purpose, deliverable description, and a list of four lessons, each with position, title, and status.
2. On first access, Lesson 1 status is `available` and Lessons 2, 3, and 4 status is `locked`.
3. `GET /v1/learners/me/progress/{module_id}` returns `lessons_complete: 0`, `lessons_total: 4`, `current_lesson_id` pointing to Lesson 1, and `module_status: in_progress`.
4. The Module Overview screen renders the module title, purpose, deliverable description, and lesson list with accurate status indicators.
5. The primary action on the Module Overview screen reads "Begin Lesson 1" on first access.
6. Both endpoints return 401 for unauthenticated requests.
7. Both endpoints return 404 for a module ID that does not exist.

---

## Development Tasks

---

### Database

- Create the database and verify connectivity.
- Apply the schema defined in OA-MVP-006 for the following tables: `learners`, `learning_paths`, `phases`, `modules`, `lessons`, `assignments`, `surveys`, `survey_questions`.
- Confirm all primary keys, foreign keys, unique constraints, and check constraints are in place.

---

### Content Seeding

- Seed one `learning_paths` row: Digital Marketing.
- Seed one `phases` row: Phase 0 — Foundations.
- Seed one `modules` row: Module 1 — The Digital Marketing Landscape, with deliverable description.
- Seed four `lessons` rows in position order: Lesson 1 — What Is Digital Marketing, Lesson 2 — Digital vs. Traditional Marketing, Lesson 3 — Core Digital Marketing Channels, Lesson 4 — Digital Marketing in Practice.
- Seed four `assignments` rows, one per lesson, with prompts and deliverable names sourced from each lesson file.
- Seed one `surveys` row for Module 1.
- Seed three `survey_questions` rows: one `rating` question, one required `text` question, one optional `text` question, as defined in OA-MVP-004.

---

### Backend

- Implement the Domain layer rule: a lesson is `available` if no prior lesson exists or the prior lesson's assignment has a submission in `submitted` status; otherwise `locked`; `complete` if the lesson's own assignment has a submission in `submitted` status.
- Implement the Domain layer rule: module status is `in_progress` unless all lessons are `complete`.
- Implement unit tests for both rules before implementing the Application layer.
- Implement the Authentication application service: create a learner record, establish a session, verify a session, resolve learner identity from a session.
- Implement the Learning Path application service method: retrieve module overview with learner-specific lesson statuses.
- Implement the Progress application service method: derive progress from confirmed submission events.

---

### API

- Implement `POST /v1/auth/login`, `POST /v1/auth/logout`, and `GET /v1/auth/me` as specified in OA-MVP-007.
- Implement `GET /v1/modules/{module_id}/overview` as specified in OA-MVP-007.
- Implement `GET /v1/learners/me/progress/{module_id}` as specified in OA-MVP-007.
- Implement the health check endpoint (unauthenticated).
- Implement the standard error model for all 401 and 404 responses as defined in OA-MVP-007.

---

### Frontend

- Establish the frontend project structure following OA-MVP-009: Presentation, Application, State, and API layers.
- Implement the API layer function for `GET /v1/modules/{module_id}/overview`.
- Implement the API layer function for `GET /v1/learners/me/progress/{module_id}`.
- Implement the API layer function for authentication (login, logout, current learner).
- Implement the authentication flow: verify session on load, redirect to login if absent or invalid.
- Implement the Module Overview screen with all components: module title, purpose, deliverable description, lesson list with status indicators (locked / available / complete), and primary action.
- Implement loading state and error state for the Module Overview screen.
- Implement the navigation rule: on application load, fetch progress and route to the Module Overview.

---

### Integration

- Connect the Module Overview screen to the real API endpoints.
- Verify that lesson status indicators reflect the actual data returned by the API.
- Verify that the primary action label ("Begin Lesson 1") is driven by the progress state returned by the server, not hardcoded.
- Verify that a 401 response from either endpoint triggers re-authentication on the frontend.

---

### Testing

- Unit tests: lesson status derivation rule (all three states: locked, available, complete).
- Unit tests: module status derivation rule (in_progress and complete transitions).
- Unit tests: authentication session establishment and verification.
- Integration tests: `GET /v1/modules/{module_id}/overview` — correct response shape, 401 for unauthenticated request, 404 for unknown module.
- Integration tests: `GET /v1/learners/me/progress/{module_id}` — correct response shape, 401 for unauthenticated request, 404 for unknown module.
- Manual acceptance: authenticate as a pilot learner, view Module Overview, confirm lesson statuses are correct.

---

### Documentation

- Authentication endpoints are defined in OA-MVP-007 (v1.2.0) per ADR-0006.
- Update OA-MVP-010 to note that Step 3 (Authentication) uses Sanctum SPA session authentication per ADR-0006.
- Record the sprint outcome in this file under a Results section once the sprint is closed.

---

## Definition of Done

A feature in this sprint is complete when all of the following are true:

**Code Complete:** All backend and frontend code for the feature has been written and reviewed according to the Code Review Rules in OA-MVP-010.

**Tests Pass:** All unit tests for Domain layer business logic pass. All integration tests for the API endpoints pass against the real database. No existing tests are broken.

**API Integrated:** The Module Overview screen consumes the real API endpoints, not mocks. The contracts defined in OA-MVP-007 are honoured by both sides.

**UI Integrated:** The Module Overview screen renders correctly in a browser in all states: default (first visit), loading, and error. All lesson statuses display correctly.

**Manual Verification Complete:** A team member who did not write the code has walked through the authentication flow and the Module Overview manually and confirmed they behave as specified in OA-MVP-003.

**Documentation Updated:** If any deviation from an approved architecture document was required, that document has been updated and the change recorded before the sprint is marked done.

---

## Risks

**Risk 1 — Authentication endpoint contract resolved**
The authentication endpoints are now defined in OA-MVP-007 (v1.2.0) per ADR-0006. This risk is resolved.

**Risk 2 — Content seeding is underestimated**
Lesson content is long and structured. Seeding errors (missing assignments, incorrect positions, wrong content) will cause incorrect lesson status display and cascade into every downstream feature. Seeding must be verified independently before backend development depends on it.

**Risk 3 — Domain rule for lesson status is more complex than it appears**
The lesson status rule depends on reading submission state, which in Sprint 1 does not yet exist for any lesson. The rule must handle the zero-submission case correctly and be tested against it explicitly.

**Risk 4 — Frontend and backend develop in isolation**
If the frontend builds against a mock API and the backend is built in parallel without continuous integration checks, the two may diverge before the Integration tasks run. Both sides should reference OA-MVP-007 as the single source of truth.

---

## Exit Criteria

Sprint 1 is complete when all of the following are true:

1. The database schema for all Sprint 1 tables is applied and verified.
2. All four lessons, four assignments, and one survey are seeded with content matching the published lesson files.
3. A learner can authenticate and establish a valid session that is accepted by all protected endpoints.
4. `GET /v1/modules/{module_id}/overview` returns the correct response for an authenticated learner, with Lesson 1 as `available` and Lessons 2–4 as `locked`.
5. `GET /v1/learners/me/progress/{module_id}` returns the correct response for an authenticated learner with no prior submissions.
6. The Module Overview screen renders with the correct module title, purpose, deliverable description, and lesson list with accurate status indicators.
7. The primary action on the Module Overview screen reads "Begin Lesson 1" on first access.
8. All unit tests and integration tests for Sprint 1 pass.
9. Manual acceptance has been completed by a team member who did not write the code.
10. OA-MVP-007 includes the authentication endpoints per ADR-0006.

---

## Sprint Log

**Date:** 2026-07-18

### Documentation Reconciliation

- Documentation reconciliation initiated under OA-REV-003.
- OA-REV-003 documentation changes applied to: MVP_DATABASE_SCHEMA.md, MVP_DOMAIN_MODEL.md, MVP_API_CONTRACTS.md, MVP_IMPLEMENTATION_PLAN.md.
- ADR-0005 (Identity Model) created.
- Stage 2 (Implementation Reconciliation) pending.

### Process Deviation

- VS-001 implementation began before the Foundation Stage Gate was approved.
- The deviation has been documented for Product Owner review.

### Governance Reminder

- Future Vertical Slices must not begin implementation before the previous Stage Gate has been approved.

---

**Date:** 2026-07-19

### Authentication Contract Reconciliation (Stage 1)

- The Product Owner selected Sanctum SPA cookie-based authentication for the first-party v0.1 SPA.
- ADR-0006 (Authentication Strategy) created to record this decision.
- The authentication transport decision from OA-REV-003 F-5 has been documented.
- The API authentication contract (OA-MVP-007) has been reconciled: Bearer-token requirement for the first-party SPA removed; Sanctum stateful SPA authentication documented; Authentication Contract Gap section resolved.
- VS-001 authentication endpoints (Login, Logout, Current Learner) documented in OA-MVP-007.
- Error status code semantics (401, 403, 404, 422) clarified in OA-MVP-007 per OA-REV-003 F-7.
- Related documentation updated for consistency: MVP_BACKEND_ARCHITECTURE.md, MVP_FRONTEND_ARCHITECTURE.md, MVP_IMPLEMENTATION_PLAN.md, SPRINT-001.md.
- Stage 2 (Implementation Reconciliation) remains pending.
- VS-002 is not authorized — requires F-5 through F-7 closure via implementation reconciliation.

---

**Date:** 2026-07-20

### OA-REV-003 Final Gate Remediation — Stage 1 (Documentation Reconciliation)

- Final Gate Review of oravil-academy-platform PR #8 identified the following documentation/governance reconciliation items.

**Completed:**

- Stale authentication wording in SPRINT-001.md Scope section corrected: Step 3 previously read "token issuance and verification"; updated to "Sanctum SPA session establishment and verification per ADR-0006". This wording conflicted with ADR-0006 and the approved Sanctum SPA cookie/session strategy. Fix applied in this entry.

**Pending — Product Owner Action Required:**

- OA-DIR-001: Not present anywhere in the authoritative documentation repository (oravil/oravil-academy). No file at any path carries the Document ID OA-DIR-001. The document has not been reconstructed or invented. Product Owner must supply and authorize OA-DIR-001 before it can be published.

- CSRF failure semantics: OA-MVP-007 (v1.2.0) defines CSRF protection as part of the Sanctum SPA flow and specifies that the frontend must obtain the CSRF cookie before login. However, the contract does not explicitly define the HTTP status code or error envelope for an invalid or expired CSRF token submitted to a state-changing endpoint. The following decisions require Product Owner input before they can be recorded in OA-MVP-007:
  - HTTP status for an invalid/expired CSRF token (419, 403, or another value).
  - Whether the CSRF failure response must conform to the universal error envelope defined in OA-MVP-007, or is handled differently (e.g. as a Laravel framework-level response).
  - Stable `code` value in the error envelope for CSRF failures, if the envelope applies.

**Governance confirmations as of this entry:**

- OA-REV-003 remains open.
- PR #8 (oravil-academy-platform) remains unapproved.
- VS-002 remains unauthorized.
- No application code, migrations, CI, or schema was modified in this remediation task.

---

**Date:** 2026-07-20

### OA-REV-003 Final Gate Remediation — Stage 1B Completion

- OA-DIR-001 was published at the verified authoritative path `docs/directives/OA-DIR-001.md` in commit `aa6bfed14d153601552d9fe04b41c057eeb021ad`.
- The Product Owner-approved CSRF failure semantics were recorded in OA-MVP-007 v1.3.0: invalid or expired CSRF tokens return HTTP 419 using the existing standard error envelope with stable code `CSRF_TOKEN_MISMATCH`.
- Documentation remediation for the OA-DIR-001 publication and CSRF contract Final Gate blockers is complete.
- Platform implementation remediation remains pending.
- PR #8 remains unapproved pending platform code remediation and repeat Final Gate Review.
- OA-REV-003 remains open.
- VS-002 remains unauthorized.

---

**Date:** 2026-07-20

### OA-REV-003 Final Closure Verification

**Platform PR #8 — Merged:**

- PR #8 (oravil/oravil-academy-platform — "Reconcile auth foundation to Learner + Sanctum SPA session flow") was merged to `main` by the Product Owner (oravil) at 2026-07-20T19:48:20Z.
- PR HEAD SHA at merge: `1e0cf70152c917aa0bbcd48d1e2200e30e86dd2d`.
- Stage 2 platform implementation reconciliation is confirmed present in the merged code (Learner identity, learners table, Sanctum SPA session auth, no localStorage, Zod login validation, TanStack Query for /me, CSRF 419 contract, PHP ^8.4, CI with PostgreSQL/migrations/Pint/PHPStan/Pest).

**CI Status at PR HEAD (1e0cf70152c917aa0bbcd48d1e2200e30e86dd2d):**

- Backend Quality: PASS (conclusion: success, run 29773386490)
- Frontend Quality: PASS (conclusion: success, run 29773386490)
- Commitlint: PASS (conclusion: success, run 29773386490)

**M-3 BOM Remediation — Completed:**

- UTF-8 BOM (EF BB BF) removed from `academy/learning-paths/digital-marketing/phase-0/module-1/lesson-01-what-is-digital-marketing.md`.
- Final repository-wide BOM scan returned zero files.

**M-1 — Unresolved (Documentation Blocker):**

- OA-MVP-006 is at version 1.1.0 and does not document the framework-managed `sessions` table.
- Pre-merge gate requires OA-MVP-006 to document the sessions table as infrastructure (not a domain entity) and be at v1.2.0 or later.
- This correction was not completed in any prior documentation task and is not authorized within the scope of this final closure verification task.
- OA-MVP-006 sessions documentation (M-1) remains an outstanding blocker for formal OA-REV-003 closure.

**M-2 — Unresolved (Documentation Blocker):**

- ADR-0002 (docs/architecture/ADR-0002-code-repository-strategy.md) declares date "2025-07-16". Evidence: git commit e41648f created this file on 2026-07-16T06:03:39Z. The date in the document is incorrect (year 2025 should be 2026).
- ADR-0003 (docs/architecture/ADR-0003-content-language-policy.md) declares date "2025-07-16". Same commit. Same incorrect year.
- These corrections were not completed in any prior documentation task and are not authorized within the scope of this final closure verification task.
- ADR-0002 and ADR-0003 date corrections (M-2) remain outstanding blockers for formal OA-REV-003 closure.

**Governance Confirmations as of This Entry:**

- OA-REV-003 remains open pending Product Owner formal closure recording.
- PR #8 has been merged to main by the Product Owner. It is no longer "unmerged".
- VS-002 remains unauthorized until M-1 and M-2 are resolved, OA-REV-003 formal closure is recorded, and Product Owner authorization is granted.
- No platform code, migrations, CI, or schema was modified in this task.
