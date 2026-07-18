# MVP Implementation Plan

**Document ID:** OA-MVP-010
**Version:** 1.1.0
**Status:** Draft — Pending Product Owner Approval
**Date:** 2026-07-16
**Reference:** OA-MVP-001 through OA-MVP-009

---

> **Review Note:** This document was updated as part of OA-REV-003 on 2026-07-18. The two overlapping Definition of Done sections have been consolidated into one canonical `## Vertical Slice Definition of Done` section with all eleven required criteria.

---

## Purpose

This document defines how development of Version 0.1 proceeds from architecture to working software. It specifies the implementation sequence, definition of done, testing strategy, code review rules, and release process. It does not introduce new features or modify any approved architecture document.

---

## Development Principles

1. Vertical slices before horizontal layers — each increment delivers a complete, working feature from database to UI before the next feature begins.
2. One feature completed before starting another — no feature is left partially implemented while a new one starts.
3. Backend and frontend evolve together — the API endpoint and the screen that consumes it are built and integrated within the same increment.
4. Working software at every step — each completed increment is deployable and demonstrable; the codebase is never in a broken state between increments.
5. Architecture documents are the specification — no implementation decision overrides an approved architecture document without a documented change.
6. API contracts are the boundary — the backend and frontend teams use OA-MVP-007 as the interface contract; neither side assumes what the other will provide beyond what is specified there.
7. No premature generalization — implementation serves the current increment only; abstractions are introduced only when a second use case demands them.
8. Every feature ends with integration testing — a feature is not done until the frontend and backend are verified working together against the real API, not a mock.
9. Schema changes are explicit — any change to the database schema is reflected in OA-MVP-006 before implementation proceeds.
10. Domain rules are tested first — business logic in the Domain layer is covered by unit tests before the Application or API layer is built on top of it.

---

## Development Order

Each step is a vertical slice that produces working, integrated software. Steps are completed in order. Step N does not begin until Step N−1 satisfies its Definition of Done.

**Step 1 — Project Foundation**
Establish the repository structure, database connection, authentication token verification, and a health check endpoint. This step produces no learner-facing feature but confirms that the technical foundation is operational before any feature work begins.

**Step 2 — Content Seeding**
Seed the database with the learning path, phase, module, lessons, assignments, and survey for Phase 0, Module 1 as defined in OA-MVP-006. This step confirms that all content is correctly structured and queryable before any feature depends on it.

**Step 3 — Authentication**
Implement the learner authentication flow: token issuance on access and token verification on every subsequent request. This step produces a learner who can authenticate and receive a token that is accepted by the API.

**Step 4 — Module Overview**
Implement `GET /v1/modules/{module_id}/overview` and `GET /v1/learners/me/progress/{module_id}`. Implement the Module Overview screen consuming both endpoints. This step produces a working Module Overview that reflects the learner's current progress state.

**Step 5 — Lesson View**
Implement `GET /v1/lessons/{lesson_id}`. Implement the Lesson View screen. Enforce the locked/available/complete access rule at both the API and frontend navigation layers. This step produces a readable lesson with correct access gating.

**Step 6 — Assignment Submission**
Implement `POST /v1/assignments/{assignment_id}/submissions`. Implement the Assignment Submission screen including client-side validation, server-side validation error display, and post-submission navigation. This step produces a working submission flow that unlocks the next lesson on success.

**Step 7 — Module Completion**
Implement `GET /v1/modules/{module_id}/completion`. Implement the Module Complete screen. Verify that the module transitions to complete after the fourth assignment is submitted and that the Module Complete screen is reachable only at that point. This step produces a working completion gate.

**Step 8 — Survey**
Implement `GET /v1/modules/{module_id}/survey` and `POST /v1/surveys/{survey_id}/responses`. Implement the Post-Module Survey screen including all question types and submission. This step produces a working survey flow and closes the learner's engagement with Version 0.1.

**Step 9 — End-to-End Verification**
Complete a full end-to-end pass of the learner journey: from first access through module completion and survey submission. Verify all navigation rules, gating conditions, and error states defined in OA-MVP-003 and OA-MVP-007.

---

## Vertical Slice Definition of Done

Each Vertical Slice must satisfy all of the following before it is considered complete:

- [ ] Scope exactly matches the declared slice.
- [ ] Documentation is updated before implementation when a contract, schema, architecture decision, or business rule changes.
- [ ] API implementation matches the approved API contract.
- [ ] Database implementation matches the approved schema.
- [ ] Tests pass locally.
- [ ] Tests pass in CI.
- [ ] Static analysis passes.
- [ ] Build passes.
- [ ] Code review is completed.
- [ ] PR contains no undeclared functionality.
- [ ] No unresolved architectural drift exists within the slice scope.

---

## Testing Strategy

### Unit Testing

Domain layer business rules are unit-tested in isolation. Tests cover: lesson unlock logic, module completion determination, submission lifecycle validation, and survey access conditions. Application service orchestration is unit-tested with the Persistence layer replaced by a test double.

### Integration Testing

Each API endpoint is integration-tested against the real database with seeded test data. Tests cover: correct response shapes, correct status codes for all documented success and error scenarios, and correct enforcement of gating rules (locked lessons return 403, duplicate submissions return 403).

### End-to-End Testing

The complete learner journey from authentication through survey submission is covered by at least one end-to-end test that exercises the full stack. Edge cases covered: direct URL access to a locked lesson, direct URL access to the Module Complete screen before all assignments are submitted, duplicate survey submission attempt.

### Manual Acceptance Testing

Each completed step in the Development Order is manually verified against the acceptance criteria derived from OA-MVP-003, OA-MVP-004, and OA-MVP-007 before the next step begins. The final end-to-end pass in Step 9 is conducted by a team member outside the development team where possible.

---

## Code Review Rules

1. No business rules in the API layer or Persistence layer.
2. No domain logic duplicated between layers.
3. No API endpoint change without a corresponding update to OA-MVP-007.
4. No database schema change without a corresponding update to OA-MVP-006.
5. No Presentation component that calls the API layer directly.
6. No Presentation component that contains navigation logic.
7. No Application layer function that renders UI elements.
8. No optimistic state update — server state must be re-fetched after every mutation.
9. No raw error detail exposed in any API response or UI message.
10. No hardcoded content IDs or lesson counts — all content structure must be sourced from the database.
11. No authentication token accessed outside the API layer.
12. No validation rule applied in the frontend that is not also enforced by the backend.
13. Every new public API endpoint must have integration tests covering all documented error codes before merge.
14. Every Domain layer rule change must have a corresponding unit test before merge.
15. Pull requests that touch more than one step of the Development Order require explicit justification.

---

## Release Strategy

Version 0.1 is released to a defined group of pilot learners after Step 9 is complete and all Definition of Done criteria are satisfied for every step.

The release process is:

1. All features satisfy the Definition of Done.
2. A final end-to-end manual verification is conducted on the production environment by a team member.
3. Pilot learners receive access credentials through the delivery channel established for the MVP.
4. A testing window is defined — a fixed period during which learners are expected to complete the module and submit the survey.
5. Completion data and survey responses are monitored against the Success Criteria defined in OA-MVP-001.
6. The testing window closes at the defined date or when all pilot learners have submitted the survey, whichever comes first.
7. Data is reviewed against the Success Criteria. If criteria are met, Version 0.2 planning begins. If criteria are not met, the methodology or content is revised and a second pilot round is conducted before Version 0.2 is approved.

No partial access is granted. No pilot learner receives access before the full module is available and verified.

---

## Risks

**Risk 1 — Pilot learners do not complete the module**
Mitigation: Keep the pilot group small (10–20 learners) and personally recruited. Establish a direct feedback channel so drop-off reasons can be identified quickly. Do not expand the pilot until the drop-off cause is understood.

**Risk 2 — Assignment submission quality is too low to assess methodology**
Mitigation: Review the first three submissions after the pilot begins. If quality is consistently below expectation, pause and investigate whether the assignment prompt or lesson content is the cause before the testing window closes.

**Risk 3 — API contract divergence between backend and frontend**
Mitigation: OA-MVP-007 is the single source of truth. Both sides are reviewed against it before Step 9 begins. Any deviation found during integration is treated as a blocker, not a workaround.

**Risk 4 — Navigation gating fails and learners skip content**
Mitigation: Navigation gating is enforced at both the frontend route level and the API level. End-to-end tests in Step 9 explicitly cover direct URL access to gated screens. A learner who bypasses the frontend cannot bypass the API.

**Risk 5 — Scope creep before release**
Mitigation: Any feature request that is not in OA-MVP-001 In Scope is recorded and deferred to Version 0.2. No new feature is added to Version 0.1 after Step 1 begins.

---

## Future Expansion

Version 0.2 planning begins only after the Version 0.1 testing window closes and the Success Criteria defined in OA-MVP-001 are met. The first decision in Version 0.2 planning is whether to expand content (add Module 2) or expand capability (add features to the existing module experience) based on what the Version 0.1 data reveals.

If the methodology is validated, Module 2 content production begins in parallel with any Version 0.2 infrastructure work. The architecture defined in OA-MVP-008 and OA-MVP-009 requires no structural changes to support additional modules — new content is seeded and the existing screens and endpoints serve it without modification.

If the methodology requires revision, content changes are made first and a second pilot is run before any capability expansion is scoped.
