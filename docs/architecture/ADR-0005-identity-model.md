# ADR-0005 — Identity Model

**Document ID:** OA-ADR-0005
**Version:** 1.0.0
**Status:** Accepted
**Date:** 2026-07-18
**Reference:** OA-REV-003, OA-MVP-006 MVP Database Schema

---

## Status

Accepted

---

## Context

Oravil Academy Version 0.1 is an MVP designed to validate a learning methodology with a small cohort of manually recruited pilot learners. The system requires an authenticated identity to associate learning progress, assignment submissions, and survey responses with a specific individual.

The scope of Version 0.1 requires exactly one authenticated role. Multiple roles, self-registration, and password recovery workflows are explicitly outside the MVP scope.

---

## Decision

Exactly one authenticated role exists in Version 0.1: **Learner**.

Learners are provisioned manually by the system operator. There is no self-registration flow.

The identity table is named `learners` and contains the following columns:

| Column | Type | Nullable | Default |
|---|---|---|---|
| id | uuid | No | gen_random_uuid() |
| email | text | No | — |
| display_name | text | No | — |
| password_hash | text | No | — |
| enrolled_at | timestamptz | No | now() |

---

## Non-Goals

The following are explicitly deferred and are not part of Version 0.1:

- **Admin role** — No administrative interface or admin-authenticated API is defined.
- **Instructor role** — No instructor interface or instructor-authenticated API is defined.
- **RBAC (Role-Based Access Control)** — No permission system, role hierarchy, or access matrix is defined.
- **Permissions model** — No granular permission assignments exist.
- **Role hierarchy** — No concept of role inheritance or elevation is defined.
- **Learner self-registration (Register flow)** — Learners cannot create their own accounts.
- **Forgot Password flow** — No password reset mechanism is provided.
- **Email verification** — No email verification step exists on account creation or authentication.

---

## Consequences

### Included

- The `learners` table stores credentials directly: `password_hash` holds the hashed credential.
- Authentication is performed against the `learners` table using `email` and the verified `password_hash`.
- Token issuance on successful authentication is the only authentication outcome.

### Excluded

- No `password_reset_tokens` table exists or is required.
- No `remember_token` column exists on any table.
- No `email_verified_at` column exists on any table.
- No `users` table exists. The `learners` table is the sole identity table.

### Future Implications

- Introducing additional roles will require a new ADR and schema amendment.
- Introducing self-registration will require a new ADR, schema amendment, and email infrastructure decision.
- Introducing password reset will require a new ADR and the addition of a `password_reset_tokens` table or equivalent mechanism.

---
