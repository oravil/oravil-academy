# CONTENT LIFECYCLE

> **Document ID:** OA-GOV-001
> **Version:** 1.0.0
> **Status:** Active
> **Owner:** Oravil Academy

---

# 1. Purpose

This document defines the official content production lifecycle for Oravil Academy. It governs how content moves from a backlog item to a published lesson, and establishes the workflow, roles, quality gates, and definition of done that all contributors — human and AI — must follow.

---

# 2. Principles

- **Single Source of Truth.** Every artifact exists in exactly one canonical location in the repository.
- **Brief Before Production.** No lesson may be written without an approved Lesson Brief. No Lesson Brief may be written without an approved Module Brief.
- **Review Before Approval.** Every artifact must complete all assigned review stages before it may be approved.
- **No Direct Lesson Creation.** Lessons are never created ad hoc. All lessons originate from a Lesson Brief that originates from a Module Brief that originates from the backlog.
- **Incremental Production.** Content is produced module by module, in phase order, within a single learning path at a time.
- **Continuous Improvement.** Process changes are tracked exclusively through `PRODUCT_BACKLOG.md`. This document changes only when the lifecycle itself changes.

---

# 3. Content Hierarchy

```
Learning Path
    ↓
Phase
    ↓
Module
    ↓
Module Brief
    ↓
Lesson Brief
    ↓
Lesson
    ↓
Review
    ↓
Approval
    ↓
Published Content
```

No stage may be skipped. Every artifact must exist before the next is created.

---

# 4. Production Workflow

## Backlog

**Purpose:** Maintain a prioritized list of all content items to be produced.

**Owner:** Product Owner

**Output:** A prioritized backlog entry in `docs/PRODUCT_BACKLOG.md` specifying the target learning path, phase, and module.

---

## Module Brief

**Purpose:** Define the scope, purpose, outcomes, lesson sequence, deliverable, and exit criteria for a single module before any lesson work begins.

**Owner:** Curriculum Architect

**Output:** `MODULE_BRIEF.md` filed under the target module directory.

**Exit Criteria:** The Module Brief is approved and committed before any Lesson Brief in the module is created.

---

## Lesson Brief

**Purpose:** Define the blueprint for a single lesson: objectives, key concepts, business context, deliverable, assessment method, and dependencies.

**Owner:** Curriculum Architect

**Output:** `lesson-[nn]-brief.md` filed under the target module directory.

**Exit Criteria:** The Lesson Brief is approved and committed before lesson production begins.

---

## Lesson Production

**Purpose:** Produce the lesson content in full compliance with `LESSON_STANDARD.md`, derived entirely from the approved Lesson Brief.

**Owner:** Content Author

**Output:** `lesson-[nn]-[slug].md` filed under the target module directory.

**Exit Criteria:** The lesson covers all objectives in the Lesson Brief, meets all structural requirements of `LESSON_STANDARD.md`, and is submitted for review.

---

## Review

**Purpose:** Verify the lesson meets quality standards before approval.

**Owner:** Technical Reviewer and Educational Reviewer

**Checklist:**

- All Lesson Brief objectives are addressed.
- Lesson structure complies with `LESSON_STANDARD.md`.
- No factual errors or outdated information.
- Language is clear, professional, and free of filler.
- Deliverable is clearly defined and achievable.
- Assessment method is measurable.
- No content from other lessons is duplicated.

**Exit Criteria:** All checklist items are confirmed. All reviewer comments are resolved.

---

## Approval

**Purpose:** Formally authorize the lesson for publication.

**Owner:** Product Owner

**Approval Rules:**

- Both Technical and Educational Reviews must be completed with no open issues.
- The lesson must comply with the Academy Constitution.
- Approval is documented in the commit message or pull request.

---

## Publication

**Purpose:** Commit the approved lesson to the repository and mark it as published.

**Owner:** Product Owner or delegated contributor

**Deliverables:**

- Lesson file committed to the correct module directory.
- Module Brief updated to reflect lesson status if applicable.
- Phase README reflects current production state.

---

# 5. Roles and Responsibilities

| Role | Responsibilities |
|---|---|
| **Product Owner** | Maintains the backlog. Approves Module Briefs, Lesson Briefs, and finished lessons. Authorizes publication. |
| **Curriculum Architect** | Designs phase and module architecture. Authors Module Briefs and Lesson Briefs. Ensures structural coherence across the learning path. |
| **Content Author** | Produces lesson content from approved Lesson Briefs. Follows `LESSON_STANDARD.md` in full. |
| **Technical Reviewer** | Verifies factual accuracy, technical correctness, and currency of all lesson content. |
| **Educational Reviewer** | Verifies instructional design quality: objective alignment, clarity, progression, and assessment validity. |
| **AI Agent** | May produce Module Briefs, Lesson Briefs, and lesson drafts when operating under an approved task. Must follow all lifecycle rules. May not approve its own output. |

---

# 6. Quality Gates

Every lesson must pass all four gates in sequence. No gate may be skipped or combined.

| Gate | Verified By |
|---|---|
| Module Brief Review | Curriculum Architect + Product Owner |
| Technical Review | Technical Reviewer |
| Educational Review | Educational Reviewer |
| Final Approval | Product Owner |

A lesson that fails any gate returns to the previous stage. It may not advance until all issues are resolved.

---

# 7. Git Workflow

Every completed artifact follows this sequence:

```
Create
    ↓
Review
    ↓
Approve
    ↓
Commit
    ↓
Push
```

- One artifact per commit is preferred.
- Commit messages must identify the artifact type, learning path, phase, module, and lesson where applicable.
- No unapproved content may be committed to `main`.

---

# 8. Definition of Done

**Module Brief**
- Covers all required sections per the Module Brief standard.
- Approved by Product Owner.
- Committed to the correct module directory.

**Lesson Brief**
- Covers all required sections per the Lesson Brief standard.
- Derived from an approved Module Brief.
- Approved by Product Owner.
- Committed to the correct module directory.

**Lesson**
- Derived from an approved Lesson Brief.
- Complies fully with `LESSON_STANDARD.md`.
- Passes all four quality gates.
- Committed and pushed to `main`.

**Module**
- All lessons in the module are complete per the Lesson definition of done.
- Module Brief reflects final state.
- Phase README updated.

---

# 9. Future Improvements

All proposed improvements to this lifecycle are tracked exclusively in `docs/PRODUCT_BACKLOG.md`.

This document is stable by design. It should change only when a structural change to the production lifecycle is approved by the Product Owner. Incremental process notes, tooling changes, and style decisions are not grounds for updating this document.
