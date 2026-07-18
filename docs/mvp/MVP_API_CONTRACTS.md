# MVP API Contracts

**Document ID:** OA-MVP-007
**Version:** 1.1.0
**Status:** Draft — Pending Product Owner Approval
**Date:** 2026-07-16
**Reference:** OA-MVP-002 MVP Information Architecture, OA-MVP-003 MVP User Flows, OA-MVP-004 MVP Wireframes, OA-MVP-005 MVP Domain Model, OA-MVP-006 MVP Database Schema

---

> **Review Note:** This document was updated as part of OA-REV-003 on 2026-07-18. An Authentication Contract Gap section has been added to document the undefined token issuance endpoint. See the Authentication Contract Gap section below.

---

## Purpose

This document defines the API contracts between the frontend and backend for Version 0.1 of Oravil Academy. It specifies every endpoint, request shape, response shape, status codes, and validation rules. It does not describe implementation, framework selection, or infrastructure.

---

## Design Principles

1. Resource-oriented — endpoints address domain resources directly; actions are expressed through HTTP methods and resource state rather than verb-based routes.
2. Predictable responses — every endpoint returns a consistent envelope structure regardless of success or failure.
3. Explicit validation — all request validation rules are defined here and enforced at the API boundary before any business logic executes.
4. Stateless requests — every request carries sufficient context to be processed independently; no session state is maintained server-side.
5. Consistent error handling — all errors follow a single error model; the frontend does not need to handle different error shapes per endpoint.

---

## Authentication

Version 0.1 uses token-based authentication. Every request to a protected endpoint must include a bearer token in the Authorization header. The token identifies the authenticated learner. Token issuance and expiry are outside the scope of this document. All endpoints in this document are protected unless stated otherwise. A request with a missing, invalid, or expired token receives a 401 response. The token issuance endpoint is not yet defined in this contract. See Authentication Contract Gap section below.

---

## Authentication Contract Gap — Product Owner Decision Required

**⚠ Product Owner Decision Required**

The token issuance endpoint is not defined in the current approved contract (OA-MVP-007). Implementation of the authentication step (Step 3 in OA-MVP-010) cannot proceed until this contract is approved.

The following contract elements must be decided before implementation can begin:

1. **Endpoint path and HTTP method** — What is the route and HTTP method for the token issuance endpoint?
2. **Request body fields** — What credentials does the learner supply in the request body?
3. **Success response envelope** — What does a successful authentication response return?
4. **Success HTTP status code** — What HTTP status code is returned on successful authentication?
5. **Authentication failure HTTP status code** — 401 is the documented expectation for invalid credentials; however, the full error response envelope requires approval before it is finalised.
6. **Token lifetime and expiry behaviour** — What is the token lifetime, and is expiry communicated in the response or managed out-of-band?

---

## Endpoints

---

### GET /v1/modules/{module_id}/overview

**Purpose:** Returns the module title, purpose, deliverable description, and the full lesson list with per-lesson completion status for the authenticated learner.

**Request Body:** None

**Response Body:**

```json
{
  "module_id": "string",
  "title": "string",
  "purpose": "string",
  "deliverable_description": "string",
  "lessons": [
    {
      "lesson_id": "string",
      "position": "integer",
      "title": "string",
      "status": "string"
    }
  ],
  "module_status": "string"
}
```

`lessons[].status` values: `locked`, `available`, `complete`

`module_status` values: `in_progress`, `complete`

**Success Status:** 200

**Possible Errors:** 401 Unauthorized, 404 Not Found

---

### GET /v1/lessons/{lesson_id}

**Purpose:** Returns the full content of a single lesson. The lesson must be in `available` or `complete` status for the authenticated learner. Locked lessons return 403.

**Request Body:** None

**Response Body:**

```json
{
  "lesson_id": "string",
  "module_id": "string",
  "position": "integer",
  "title": "string",
  "estimated_reading_minutes": "integer",
  "content": "string",
  "assignment": {
    "assignment_id": "string",
    "deliverable_name": "string",
    "prompt": "string",
    "minimum_word_count": "integer"
  }
}
```

`estimated_reading_minutes` may be null if not defined.

`assignment.minimum_word_count` may be null if no minimum is defined for the lesson.

**Success Status:** 200

**Possible Errors:** 401 Unauthorized, 403 Forbidden (lesson is locked), 404 Not Found

---

### POST /v1/assignments/{assignment_id}/submissions

**Purpose:** Creates an assignment submission for the authenticated learner. A learner may only submit once per assignment. Submission unlocks the next lesson or, if this is the final assignment, transitions the module to complete.

**Request Body:**

```json
{
  "content": "string"
}
```

**Response Body:**

```json
{
  "submission_id": "string",
  "assignment_id": "string",
  "status": "string",
  "submitted_at": "string"
}
```

`status` value in Version 0.1: `submitted`

`submitted_at` is an ISO 8601 timestamp.

**Success Status:** 201

**Possible Errors:** 401 Unauthorized, 403 Forbidden (assignment not yet unlocked, or already submitted), 404 Not Found, 422 Unprocessable Entity (validation failure)

---

### GET /v1/learners/me/progress/{module_id}

**Purpose:** Returns the authenticated learner's current progress through the specified module, derived from confirmed submission events.

**Request Body:** None

**Response Body:**

```json
{
  "module_id": "string",
  "lessons_complete": "integer",
  "lessons_total": "integer",
  "current_lesson_id": "string",
  "module_status": "string",
  "survey_submitted": "boolean"
}
```

`current_lesson_id` is null when the module is complete and no further lesson is pending.

`module_status` values: `in_progress`, `complete`

**Success Status:** 200

**Possible Errors:** 401 Unauthorized, 404 Not Found

---

### GET /v1/modules/{module_id}/completion

**Purpose:** Returns the module completion summary for the authenticated learner, including the list of completed lessons. Only accessible when the module status is `complete`. Returns 403 if the module is not yet complete.

**Request Body:** None

**Response Body:**

```json
{
  "module_id": "string",
  "title": "string",
  "deliverable_description": "string",
  "completed_lessons": [
    {
      "lesson_id": "string",
      "position": "integer",
      "title": "string"
    }
  ],
  "survey_submitted": "boolean"
}
```

**Success Status:** 200

**Possible Errors:** 401 Unauthorized, 403 Forbidden (module not yet complete), 404 Not Found

---

### GET /v1/modules/{module_id}/survey

**Purpose:** Returns the survey for the specified module, including all questions in display order. Only accessible when the module status is `complete` and the learner has not yet submitted a survey response. Returns 403 if the module is not yet complete or the survey has already been submitted.

**Request Body:** None

**Response Body:**

```json
{
  "survey_id": "string",
  "module_id": "string",
  "title": "string",
  "questions": [
    {
      "survey_question_id": "string",
      "position": "integer",
      "question_text": "string",
      "question_type": "string",
      "required": "boolean"
    }
  ]
}
```

`question_type` values: `rating`, `text`

**Success Status:** 200

**Possible Errors:** 401 Unauthorized, 403 Forbidden (module not complete, or survey already submitted), 404 Not Found

---

### POST /v1/surveys/{survey_id}/responses

**Purpose:** Submits the authenticated learner's answers to the post-module survey. All required questions must be answered. A learner may only submit once per survey.

**Request Body:**

```json
{
  "answers": [
    {
      "survey_question_id": "string",
      "answer_text": "string",
      "answer_rating": "integer"
    }
  ]
}
```

For `rating` questions: `answer_rating` is required, `answer_text` is ignored.

For `text` questions: `answer_text` is required, `answer_rating` is ignored.

**Response Body:**

```json
{
  "survey_id": "string",
  "submitted_at": "string"
}
```

`submitted_at` is an ISO 8601 timestamp.

**Success Status:** 201

**Possible Errors:** 401 Unauthorized, 403 Forbidden (module not complete, or survey already submitted), 404 Not Found, 422 Unprocessable Entity (validation failure)

---

## Error Model

All error responses use the following structure regardless of endpoint:

```json
{
  "error": {
    "code": "string",
    "message": "string",
    "fields": [
      {
        "field": "string",
        "message": "string"
      }
    ]
  }
}
```

`code` is a machine-readable string identifying the error class (for example: `validation_error`, `not_found`, `forbidden`, `unauthorized`).

`message` is a human-readable description of the error.

`fields` is present only on 422 responses and lists each field that failed validation. It is omitted on all other error types.

---

## Validation Rules

### POST /v1/assignments/{assignment_id}/submissions

- `content` is required and must not be empty or whitespace only.
- `content` must meet the word count defined in `assignments.minimum_word_count` if that value is set.
- The assignment must be in an accessible state for the learner (its preceding lesson must be complete).
- A submission must not already exist for this learner and assignment.

### POST /v1/surveys/{survey_id}/responses

- `answers` is required and must not be empty.
- Every `survey_question_id` in the request must belong to the specified survey.
- All questions where `required` is true must have a corresponding answer in the request.
- For questions with `question_type` of `rating`: `answer_rating` must be an integer between 1 and 5 inclusive.
- For questions with `question_type` of `text`: `answer_text` must not be empty or whitespace only.
- A survey response must not already exist for this learner and survey.

---

## Versioning Strategy

All routes are prefixed with `/v1/`. When a breaking change is required, a new version prefix is introduced (for example `/v2/`) and the previous version is maintained until all clients have migrated. Non-breaking additions — new optional response fields, new endpoints — do not require a version increment. Clients must tolerate unknown fields in response bodies.

---

## Future Expansion

All routes are parameterised by resource identifiers — `module_id`, `lesson_id`, `assignment_id`, `survey_id` — rather than hard-coded to Phase 0, Module 1. Adding a second module or a second learning path requires no new endpoints; the same seven routes serve all modules and all learning paths. Progress and completion endpoints are scoped per module, so a learner enrolled in multiple modules receives independent progress records per module without any structural change to the API. If a new resource type is introduced in a future version — for example an enrolment record or a certificate — it is added as a new endpoint group under the same `/v1/` prefix without modifying existing contracts.
