# MVP API Contracts

**Document ID:** OA-MVP-007
**Version:** 1.3.0
**Status:** Draft — Pending Product Owner Approval
**Date:** 2026-07-20
**Reference:** OA-MVP-002 MVP Information Architecture, OA-MVP-003 MVP User Flows, OA-MVP-004 MVP Wireframes, OA-MVP-005 MVP Domain Model, OA-MVP-006 MVP Database Schema, OA-ADR-0006 Authentication Strategy

---

> **Review Note:** This document was updated on 2026-07-20 as part of OA-REV-003 Final Gate Remediation — Stage 1B. The Product Owner-approved CSRF failure contract now defines HTTP 419, the stable error code `CSRF_TOKEN_MISMATCH`, and use of the existing standard Error Model. The prior Authentication Contract Gap remains resolved per ADR-0006.

---

## Purpose

This document defines the API contracts between the frontend and backend for Version 0.1 of Oravil Academy. It specifies every endpoint, request shape, response shape, status codes, and validation rules. It does not describe implementation, framework selection, or infrastructure.

---

## Design Principles

1. Resource-oriented — endpoints address domain resources directly; actions are expressed through HTTP methods and resource state rather than verb-based routes.
2. Predictable responses — every endpoint returns a consistent envelope structure regardless of success or failure.
3. Explicit validation — all request validation rules are defined here and enforced at the API boundary before any business logic executes.
4. Session-authenticated requests — the first-party SPA authenticates through a stateful Sanctum session; each request carries sufficient context via the session cookie to be authenticated. No bearer token is required in request headers for the first-party SPA.
5. Consistent error handling — all errors follow a single error model; the frontend does not need to handle different error shapes per endpoint.

---

## Authentication

Version 0.1 uses Laravel Sanctum stateful SPA authentication as defined in ADR-0006 (OA-ADR-0006). The first-party React SPA authenticates using the Sanctum cookie/session mechanism. Authentication state is maintained through secure HttpOnly session cookies.

All endpoints in this document are protected unless stated otherwise. Protected endpoints require an authenticated Learner session. A request with missing, invalid, or expired authentication state receives a 401 response.

The frontend does not persist bearer tokens in `localStorage`. No Authorization header with a bearer token is required for first-party SPA requests.

CSRF protection applies to state-changing authentication and session requests as required by the Sanctum SPA flow. The frontend obtains the Sanctum CSRF cookie before submitting login credentials. A state-changing request with an invalid or expired CSRF token receives a 419 response using the Error Model defined in this document and the stable error code `CSRF_TOKEN_MISMATCH`.

---

## Authentication Endpoints (VS-001)

The following endpoints support the approved v0.1 authentication flow for the first-party SPA. These endpoints are within VS-001 scope.

---

### POST /v1/auth/login

**Purpose:** Authenticates a manually provisioned Learner and establishes an authenticated Sanctum SPA session.

**Authentication:** Public — no authenticated session required. CSRF cookie must be obtained before submitting this request as required by the Sanctum SPA flow.

**Request Body:**

```json
{
  "email": "string",
  "password": "string"
}
```

**Response Body:**

```json
{
  "learner_id": "string",
  "email": "string",
  "display_name": "string"
}
```

**Success Status:** 200

**Possible Errors:**

- 401 Unauthorized — invalid credentials (email does not match a provisioned Learner, or password is incorrect).
- 419 — invalid or expired CSRF token.
- 422 Unprocessable Entity — request validation failure (missing or malformed fields).

All error responses use the Error Model defined in this document.

---

### POST /v1/auth/logout

**Purpose:** Invalidates the authenticated Learner's session and ends the Sanctum SPA session.

**Authentication:** Required — authenticated Learner session.

**Request Body:** None

**Response Body:** None (empty response body)

**Success Status:** 204

**Possible Errors:**

- 401 Unauthorized — no authenticated session or session is invalid/expired.
- 419 — invalid or expired CSRF token.

All error responses use the Error Model defined in this document.

---

### GET /v1/auth/me

**Purpose:** Returns the currently authenticated Learner's profile.

**Authentication:** Required — authenticated Learner session.

**Request Body:** None

**Response Body:**

```json
{
  "learner_id": "string",
  "email": "string",
  "display_name": "string"
}
```

No credential material (e.g. `password_hash`) is included in the response.

**Success Status:** 200

**Possible Errors:**

- 401 Unauthorized — no authenticated session or session is invalid/expired.

All error responses use the Error Model defined in this document.

---

## Content Endpoints

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

**Purpose:** Creates an assignment submission for the authenticated learner. A learner may only submit once per assignment. Submission unlocks the next lesson or, if this is the final assignment, completes the module.

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

**Possible Errors:** 401 Unauthorized, 403 Forbidden (assignment not yet unlocked, or already submitted), 404 Not Found, 419 (invalid or expired CSRF token), 422 Unprocessable Entity (validation failure)

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

**Purpose:** Returns the survey for the specified module, including all questions in display order. Only accessible when the module status is `complete` and the learner has not yet submitted a survey response.

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

**Possible Errors:** 401 Unauthorized, 403 Forbidden (module not complete, or survey already submitted), 404 Not Found, 419 (invalid or expired CSRF token), 422 Unprocessable Entity (validation failure)

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

## Error Status Code Semantics

### 401 Unauthorized

Used when authentication is missing, invalid, or expired. This includes:

- No authenticated session present.
- Session is invalid or expired.
- Invalid login credentials (email does not match a provisioned Learner, or password is incorrect).

All 401 responses use the Error Model defined above.

### 403 Forbidden

Used when the Learner is authenticated but is not permitted to access the requested resource or resource state. Examples:

- Attempting to access a locked lesson.
- Attempting to submit an assignment that is not yet unlocked.
- Attempting to submit an assignment that has already been submitted.
- Attempting to access the Module Complete screen before all assignments are submitted.
- Attempting to access or submit a survey before the module is complete or after the survey has already been submitted.

All 403 responses use the Error Model defined above.

### 404 Not Found

Used when the requested resource does not exist. All 404 responses use the Error Model defined above.

### 419 CSRF Token Mismatch

Used when a state-changing request contains an invalid or expired CSRF token.

All 419 responses use the Error Model defined above with:

- `error.code`: `CSRF_TOKEN_MISMATCH`
- `error.message`: `CSRF token is invalid or expired.`
- `error.fields`: omitted

The response must not expose internal session details, raw CSRF tokens, expected token values, or framework internals.

### 422 Unprocessable Entity

Used only for request validation failures — when the request body is syntactically valid but fails the validation rules defined in this contract. Examples:

- Missing required fields.
- Content below minimum word count.
- Invalid survey answer format.

All 422 responses use the Error Model defined above, including the `fields` array listing each field that failed validation.

---

## Validation Rules

### POST /v1/auth/login

- `email` is required and must be a valid email address format.
- `password` is required and must not be empty.

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

All routes are prefixed with `/v1/`. When a breaking change is required, a new version prefix is introduced (for example `/v2/`) and the previous version is maintained until all clients have migrated. Additive changes — new endpoints or new optional response fields — do not require a version increment.

---

## Future Expansion

All routes are parameterised by resource identifiers — `module_id`, `lesson_id`, `assignment_id`, `survey_id` — rather than hard-coded to Phase 0, Module 1. Adding a second module or a second learning path requires no contract changes, only new data.
