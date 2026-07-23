# MVP User Flows

**Document ID:** OA-MVP-003
**Version:** 1.1.0
**Status:** Draft — Pending Product Owner Approval
**Date:** 2026-07-23
**Reference:** OA-MVP-001 MVP Product Brief, OA-MVP-002 MVP Information Architecture, OA-MVP-006 MVP Database Schema, OA-MVP-007 MVP API Contracts

---

> **Review Note:** This document was updated on 2026-07-23 as part of OA-MVP-010 Step 9 remediation (finding S9-F-02). The exceptional flow "Learner Edits a Previously Submitted Assignment" described resubmission and submission versioning, contradicting this document's own completion rules and the one-submission-per-assignment rule defined in OA-MVP-005, OA-MVP-006, and OA-MVP-007. The flow has been corrected: a submission is final and may be viewed but not modified.

---

## Purpose

This document defines how a learner moves through Version 0.1 of Oravil Academy, from first access through module completion. It covers the primary learning flow, continuation behaviour, exceptional situations, and the conditions required to mark Module 1 as complete.

---

## Primary User

The single user type in Version 0.1 is the enrolled learner — an adult with no formal digital marketing training who has been granted access to Phase 0, Module 1. There is no administrator flow, no instructor flow, and no guest flow in scope for this version.

---

## Flow 1 — First-Time Access

1. Learner receives access credentials or an access link through the MVP delivery channel.
2. Learner opens the product and lands on the Module Overview screen.
3. Learner reads the module title, purpose, four-lesson structure, and module deliverable description.
4. Learner selects the action to begin Lesson 1.
5. Learner is taken to the Lesson View for Lesson 1.

---

## Flow 2 — Learning a Lesson

1. Learner opens the Lesson View for the current lesson.
2. Learner reads the full lesson content from top to bottom: Business Context, Learning Objectives, Core Concepts, Professional Thinking, Practical Example, Workshop, Assignment instructions.
3. Learner completes the Workshop activity within the lesson as a preparation step.
4. Learner selects the action to proceed to assignment submission.
5. Learner is taken to the Assignment Submission screen for the current lesson.
6. Learner writes and submits the assignment deliverable as specified in the lesson.
7. Learner receives confirmation that the assignment has been submitted.
8. If the completed lesson is Lesson 1, 2, or 3, the next lesson is unlocked and the learner is prompted to continue.
9. If the completed lesson is Lesson 4, the learner is directed to the Module Complete screen.

---

## Flow 3 — Continuing the Module

1. Learner returns to the product after a previous session.
2. Learner lands on the Module Overview screen, which reflects current progress.
3. The module overview indicates which lessons are complete, which lesson is current, and which lessons remain locked.
4. Learner selects the current lesson to resume.
5. Learner is taken to the Lesson View for the current lesson.
6. Learner proceeds through Flow 2 from step 2.

---

## Flow 4 — Completing the Module

1. Learner submits the Lesson 4 assignment.
2. Learner receives confirmation that the Lesson 4 assignment has been submitted.
3. The system determines that all four assignments have been submitted.
4. Learner is directed to the Module Complete screen.
5. Learner reads the completion confirmation and is prompted to proceed to the post-module survey.
6. Learner opens the Post-Module Survey screen.
7. Learner provides a satisfaction rating on a 1–5 scale.
8. Learner provides qualitative written feedback on the module experience.
9. Learner submits the survey.
10. Learner receives confirmation that the survey has been submitted and that Module 1 is complete.

---

## Exceptional Flows

### Assignment Validation Failure

If the learner submits an empty assignment or an assignment that does not meet the minimum input requirement, the submission is rejected and the learner is prompted to complete the required field before resubmitting. The lesson remains in its current state and no progress is recorded.

### Learner Leaves Before Completing a Lesson

If a learner closes the product before submitting an assignment, their progress within the lesson is not recorded as complete. On return, the learner is taken to the Module Overview showing the current lesson as incomplete. The learner re-opens the lesson and resumes reading from the beginning. No partial lesson state is saved in Version 0.1.

### Learner Returns After Several Days

The behaviour is identical to Flow 3. The Module Overview reflects the last confirmed completed state. Lessons completed in previous sessions remain accessible for re-reading. The current lesson is the first lesson for which no assignment has been submitted.

### Learner Revisits a Previously Submitted Assignment

Per the submission rules defined in OA-MVP-006 and OA-MVP-007, a learner may submit only once per assignment. Each submission is final.

Previously submitted assignments may be viewed but not modified. This rule is in place to maintain the integrity of completion data collected for methodology validation.

---

## Completion Rules

- Lesson 1 is complete when the Lesson 1 assignment has been submitted.
- Lesson 2 is unlocked only after Lesson 1 is complete.
- Lesson 2 is complete when the Lesson 2 assignment has been submitted.
- Lesson 3 is unlocked only after Lesson 2 is complete.
- Lesson 3 is complete when the Lesson 3 assignment has been submitted.
- Lesson 4 is unlocked only after Lesson 3 is complete.
- Lesson 4 is complete when the Lesson 4 assignment has been submitted.
- Module 1 is complete when all four lesson assignments have been submitted.
- The Post-Module Survey is available only after Module 1 is complete.
- The learner's engagement with Version 0.1 is closed upon survey submission.

---

## Future Expansion

The flows defined in this document are content-agnostic. Flow 2 — Learning a Lesson — applies to any lesson in any module in any learning path, provided the lesson follows the standard structure defined in OA-STD-001. Flow 4 — Completing the Module — applies to any module that ends with a final lesson and a post-module survey. When additional learning paths are introduced, no flow redesign is required. New content inherits the same sequential progression, the same assignment submission gate, and the same completion model.
