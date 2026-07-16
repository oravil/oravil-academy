# Sprint 1

**Sprint Number:** 001
**Status:** Planned
**Date:** 2026-07-16
**Reference:** OA-MVP-008 MVP Backend Architecture, OA-MVP-009 MVP Frontend Architecture, OA-MVP-010 MVP Implementation Plan, OA-GUIDE-001 Project Execution Guide

---

## Sprint Goal

Deliver a working, authenticated learner session and a fully functional Module Overview screen. At the end of Sprint 1, a learner can authenticate, receive a valid token, and view the Module Overview showing the four lessons of Module 1 with correct initial status — Lesson 1 available, Lessons 2 through 4 locked — backed by real data from the database and confirmed progress state from the API.

---

## Scope

This sprint covers two items from the Implementation Plan:

- Step 1 — Project Foundation (database, authentication infrastructure, health check)
- Step 2 — Content Seeding (Phase 0, Module 1 content in the database)
- Step 3 — Authentication (token issuance and verification)
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
2. The learner can authenticate using their credentials and receive a bearer token.
3. A request to any protected endpoint with no token receives a 401 response.
4. A request to any protected endpoint with an invalid or expired token receives a 401 response.
5. A valid token is accepted by the API and the authenticated learner identity is resolved correctly.

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
- Seed four `lessons` rows in position order: Lesson 1 — What Is Digital Marketing, Lesson 2 — Digital vs. Traditional Marketing, Lesson 3 — Core Digital Marketing Channels, Lesson 4 — Digital Marketing Careers. Full lesson content sourced from the published lesson files in `academy/learning-paths/digital-marketing/phase-0/module-1/`.
- Seed four `assignments` rows, one per lesson, with prompts and deliverable names sourced from each lesson file.
- Seed one `surveys` row for Module 1.
- Seed three `survey_questions` rows: one `rating` question, one required `text` question, one optional `text` question, as defined in OA-MVP-004.

---

### Backend

- Implement the Domain layer rule: a lesson is `available` if no prior lesson exists or the prior lesson's assignment has a submission in `submitted` status; otherwise `locked`; `complete` if the lesson's own assignment has a submission in `submitted` status.
- Implement the Domain layer rule: module status is `in_progress` unless all lessons are `complete`.
- Implement unit tests for both rules before implementing the Application layer.
- Implement the Authentication application service: create a learner record, issue a token, verify a token, resolve learner identity from a token.
- Implement the Learning Path application service method: retrieve module overview with learner-specific lesson statuses.
- Implement the Progress application service method: derive progress from confirmed submission events.

---

### API

- Implement `POST /v1/auth/token` (or equivalent access endpoint) for token issuance.
- Implement `GET /v1/modules/{module_id}/overview` as specified in OA-MVP-007.
- Implement `GET /v1/learners/me/progress/{module_id}` as specified in OA-MVP-007.
- Implement the health check endpoint (unauthenticated).
- Implement the standard error model for all 401 and 404 responses as defined in OA-MVP-007.

---

### Frontend

- Establish the frontend project structure following OA-MVP-009: Presentation, Application, State, and API layers.
- Implement the API layer function for `GET /v1/modules/{module_id}/overview`.
- Implement the API layer function for `GET /v1/learners/me/progress/{module_id}`.
- Implement the API layer function for token issuance.
- Implement the authentication flow: verify token on load, redirect to access entry if absent or invalid, attach token to all subsequent requests.
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
- Unit tests: authentication token issuance and verification.
- Integration tests: `GET /v1/modules/{module_id}/overview` — correct response shape, 401 for missing token, 404 for unknown module.
- Integration tests: `GET /v1/learners/me/progress/{module_id}` — correct response shape, 401 for missing token, 404 for unknown module.
- Manual acceptance: authenticate as a pilot learner, view Module Overview, confirm lesson statuses are correct.

---

### Documentation

- Add the authentication endpoint to OA-MVP-007 as an amendment before implementation begins.
- Update OA-MVP-010 to note that Step 3 (Authentication) requires an API contract amendment.
- Record the sprint outcome in this file under a Results section once the sprint is closed.

---

## Definition of Done

A feature in this sprint is complete when all of the following are true:

**Code Complete:** All backend and frontend code for the feature has been written and reviewed according to the Code Review Rules in OA-MVP-010.

**Tests Pass:** All unit tests for Domain layer business logic pass. All integration tests for the API endpoints pass against the real database. No existing tests are broken.

**API Integrated:** The Module Overview screen consumes the real API endpoints, not mocks. The contracts defined in OA-MVP-007 are honoured by both sides.

**UI Integrated:** The Module Overview screen renders correctly in a browser in all states: default (first visit), loading, and error. All lesson statuses display correctly.

**Manual Verification Complete:** A team member who did not write the code has walked through the authentication flow and the Module Overview manually and confirmed they behave as specified in OA-MVP-003 and OA-MVP-004.

**Documentation Updated:** If any deviation from an approved architecture document was required, that document has been updated and the change recorded before the sprint is marked done.

---

## Risks

**Risk 1 — Authentication endpoint not in OA-MVP-007**
The token issuance endpoint is not defined in the approved API contracts. Implementation cannot begin on the authentication API until OA-MVP-007 is amended. This must be resolved in the first day of the sprint.

**Risk 2 — Content seeding is underestimated**
Lesson content is long and structured. Seeding errors (missing assignments, incorrect positions, wrong content) will cause incorrect lesson status display and cascade into every downstream feature. Seeding must be verified independently before backend development depends on it.

**Risk 3 — Domain rule for lesson status is more complex than it appears**
The lesson status rule depends on reading submission state, which in Sprint 1 does not yet exist for any lesson. The rule must handle the zero-submission case correctly and be tested against it explicitly, not just against future post-submission states.

**Risk 4 — Frontend and backend develop in isolation**
If the frontend builds against a mock API and the backend is built in parallel without continuous integration checks, the two may diverge before the Integration tasks run. Both sides should reference OA-MVP-007 as the single contract and integrate at the first available opportunity.

---

## Exit Criteria

Sprint 1 is complete when all of the following are true:

1. The database schema for all Sprint 1 tables is applied and verified.
2. All four lessons, four assignments, and one survey are seeded with content matching the published lesson files.
3. A learner can authenticate and receive a valid token that is accepted by all protected endpoints.
4. `GET /v1/modules/{module_id}/overview` returns the correct response for an authenticated learner, with Lesson 1 as `available` and Lessons 2–4 as `locked`.
5. `GET /v1/learners/me/progress/{module_id}` returns the correct response for an authenticated learner with no prior submissions.
6. The Module Overview screen renders with the correct module title, purpose, deliverable description, and lesson list with accurate status indicators.
7. The primary action on the Module Overview screen reads "Begin Lesson 1" on first access.
8. All unit tests and integration tests for Sprint 1 pass.
9. Manual acceptance has been completed by a team member who did not write the code.
10. OA-MVP-007 has been amended to include the authentication endpoint.
