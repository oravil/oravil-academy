# MVP Database Schema

**Document ID:** OA-MVP-006
**Version:** 1.1.0
**Status:** Draft — Pending Product Owner Approval
**Date:** 2026-07-16
**Reference:** OA-MVP-005 MVP Domain Model, OA-MVP-002 MVP Information Architecture, OA-MVP-003 MVP User Flows, OA-MVP-004 MVP Wireframes

---

> **Review Note:** This document was updated as part of OA-REV-003 on 2026-07-18. The `learners` table identity model has been updated to include `password_hash` and remove `updated_at`. See ADR-0005 for the identity model decision record.

---

## Purpose

This document defines the PostgreSQL relational schema for Version 0.1 of Oravil Academy. It translates the approved Domain Model into tables, columns, constraints, and relationships. It does not include migration scripts, ORM models, or application code.

---

## Design Principles

1. Normalize where practical — avoid redundant data; reference by foreign key rather than duplicating values.
2. Business integrity first — all domain rules defined in OA-MVP-005 are enforced at the database level where possible.
3. Explicit foreign keys — every relationship between tables is expressed as a named foreign key constraint.
4. Immutable content records — learning content rows (learning paths, phases, modules, lessons, assignments) are append-only by convention; no soft-delete column is introduced in Version 0.1.
5. Simple before clever — no partitioning, no JSONB columns, no complex inheritance; the schema must be readable by any engineer without explanation.

---

## Tables

---

### learners

**Purpose:** Stores one record per enrolled learner.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| email | text | No | — |
| display_name | text | No | — |
| password_hash | text | No | — |
| enrolled_at | timestamptz | No | now() |

**Primary Key:** id

**Unique Constraints:** email

---

### learning_paths

**Purpose:** Stores one record per learning path. In Version 0.1, one row exists: Digital Marketing.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| slug | text | No | — |
| title | text | No | — |
| created_at | timestamptz | No | now() |
| updated_at | timestamptz | No | now() |

**Primary Key:** id

**Unique Constraints:** slug

---

### phases

**Purpose:** Stores one record per phase within a learning path.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| learning_path_id | uuid | No | — |
| slug | text | No | — |
| title | text | No | — |
| position | integer | No | — |
| created_at | timestamptz | No | now() |
| updated_at | timestamptz | No | now() |

**Primary Key:** id

**Foreign Keys:** learning_path_id → learning_paths.id

**Unique Constraints:** (learning_path_id, position), (learning_path_id, slug)

**Check Constraints:** position > 0

---

### modules

**Purpose:** Stores one record per module within a phase.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| phase_id | uuid | No | — |
| slug | text | No | — |
| title | text | No | — |
| position | integer | No | — |
| deliverable_description | text | Yes | — |
| created_at | timestamptz | No | now() |
| updated_at | timestamptz | No | now() |

**Primary Key:** id

**Foreign Keys:** phase_id → phases.id

**Unique Constraints:** (phase_id, position), (phase_id, slug)

**Check Constraints:** position > 0

---

### lessons

**Purpose:** Stores one record per lesson within a module. Lesson content is stored as structured text.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| module_id | uuid | No | — |
| slug | text | No | — |
| title | text | No | — |
| position | integer | No | — |
| content | text | No | — |
| estimated_reading_minutes | integer | Yes | — |
| created_at | timestamptz | No | now() |
| updated_at | timestamptz | No | now() |

**Primary Key:** id

**Foreign Keys:** module_id → modules.id

**Unique Constraints:** (module_id, position), (module_id, slug)

**Check Constraints:** position > 0

---

### assignments

**Purpose:** Stores one assignment per lesson. Each assignment defines the deliverable prompt and the minimum word count required for a valid submission.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| lesson_id | uuid | No | — |
| prompt | text | No | — |
| deliverable_name | text | No | — |
| minimum_word_count | integer | Yes | — |
| created_at | timestamptz | No | now() |
| updated_at | timestamptz | No | now() |

**Primary Key:** id

**Foreign Keys:** lesson_id → lessons.id

**Unique Constraints:** lesson_id — one assignment per lesson

**Check Constraints:** minimum_word_count > 0 (when not null)

---

### assignment_submissions

**Purpose:** Stores one submission record per learner per assignment. Status reflects the submission lifecycle; the only valid status in Version 0.1 is `submitted`.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| learner_id | uuid | No | — |
| assignment_id | uuid | No | — |
| content | text | No | — |
| status | text | No | 'submitted' |
| submitted_at | timestamptz | No | now() |

**Primary Key:** id

**Foreign Keys:** learner_id → learners.id, assignment_id → assignments.id

**Unique Constraints:** (learner_id, assignment_id) — one submission per learner per assignment

**Check Constraints:** status IN ('submitted')

Note: The allowed status values are intentionally version-scoped and may expand in future versions without changing the schema.

---

### surveys

**Purpose:** Stores one survey per module. In Version 0.1, one survey is associated with Module 1.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| module_id | uuid | No | — |
| title | text | No | — |
| created_at | timestamptz | No | now() |
| updated_at | timestamptz | No | now() |

**Primary Key:** id

**Foreign Keys:** module_id → modules.id

**Unique Constraints:** module_id — one survey per module

---

### survey_questions

**Purpose:** Stores each question belonging to a survey, with position controlling display order and question_type indicating the expected response format.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| survey_id | uuid | No | — |
| position | integer | No | — |
| question_text | text | No | — |
| question_type | text | No | — |
| required | boolean | No | true |
| created_at | timestamptz | No | now() |
| updated_at | timestamptz | No | now() |

**Primary Key:** id

**Foreign Keys:** survey_id → surveys.id

**Unique Constraints:** (survey_id, position)

**Check Constraints:** position > 0, question_type IN ('rating', 'text')

---

### survey_responses

**Purpose:** Stores each learner's submitted answers to a survey. One row per learner per survey_question captures the individual answer. The survey is reachable via survey_question_id → survey_questions.survey_id, eliminating the need for a redundant survey_id column.

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| learner_id | uuid | No | — |
| survey_question_id | uuid | No | — |
| answer_text | text | Yes | — |
| answer_rating | integer | Yes | — |
| submitted_at | timestamptz | No | now() |

**Primary Key:** id

**Foreign Keys:** learner_id → learners.id, survey_question_id → survey_questions.id

**Unique Constraints:** (learner_id, survey_question_id) — one answer per learner per question

**Check Constraints:** answer_rating BETWEEN 1 AND 5 (when not null)

---

## Relationships

learners has many assignment_submissions via learner_id.

learners has many survey_responses via learner_id.

learning_paths has many phases via learning_path_id.

phases belongs to one learning_path via learning_path_id.

phases has many modules via phase_id.

modules belongs to one phase via phase_id.

modules has many lessons via module_id.

modules has one survey via module_id.

lessons belongs to one module via module_id.

lessons has one assignment via lesson_id.

assignments belongs to one lesson via lesson_id.

assignments has many assignment_submissions via assignment_id.

assignment_submissions belongs to one learner via learner_id.

assignment_submissions belongs to one assignment via assignment_id.

surveys belongs to one module via module_id.

surveys has many survey_questions via survey_id.

survey_questions belongs to one survey via survey_id.

survey_responses belongs to one learner via learner_id.

survey_responses belongs to one survey_question via survey_question_id. The associated survey is resolved through survey_questions.survey_id.

---

## Integrity Rules

1. A learner's email address is unique across the system.
2. A lesson's position is unique within its module — no two lessons in the same module share a position value.
3. A module's position is unique within its phase.
4. A phase's position is unique within its learning path.
5. Each lesson has at most one assignment.
6. Each module has at most one survey.
7. A learner may submit at most one response per assignment.
8. A learner may answer each survey question at most once; survey completion is determined by comparing the learner's answers against the full question list for the relevant survey.
9. A rating answer must be between 1 and 5 inclusive.
10. Assignment submission status must be a value from the defined allowed set; in Version 0.1 this is `submitted` only.
11. Survey question type must be either `rating` or `text`.
12. Minimum word count on an assignment, when specified, must be greater than zero.

---

## Index Strategy

**learners.email** — unique index; supports login lookup by email address.

**assignment_submissions(learner_id, assignment_id)** — unique index; enforces one submission per learner per assignment and supports retrieval of all submissions for a given learner.

**assignment_submissions(assignment_id)** — supports retrieval of all submissions for a given assignment during review.

**lessons(module_id, position)** — unique index; supports ordered lesson retrieval for a given module.

**survey_responses(learner_id, survey_question_id)** — unique index; enforces one answer per learner per question and supports checking whether a learner has answered all questions in a survey when joined with survey_questions.

---

## Future Expansion

The schema is structurally generic. Adding a second learning path requires only new rows in learning_paths, phases, modules, lessons, and assignments — no schema changes. A learner's progress state is fully derivable from assignment_submissions and survey_responses filtered by learning path content, so no learner-path enrolment table is introduced in Version 0.1 but can be added later as a join table between learners and learning_paths without altering existing tables. Additional surveys for future modules require only new rows in surveys and survey_questions. Because survey_responses references survey_question_id directly rather than survey_id, adding new surveys introduces no change to the survey_responses table. The submission status check constraint on assignment_submissions can be extended in a future migration to permit additional lifecycle values without changing any other table or relationship.
