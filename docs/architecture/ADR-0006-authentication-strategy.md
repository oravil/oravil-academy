# ADR-0006 — Authentication Strategy

**Document ID:** OA-ADR-0006
**Version:** 1.0.0
**Status:** Accepted
**Date:** 2026-07-19
**Reference:** OA-REV-003 F-5, OA-REV-003 F-7, OA-ADR-0005 Identity Model, OA-MVP-007 MVP API Contracts

---

## Status

Accepted

---

## Context

Oravil Academy Version 0.1 consists of a first-party React SPA frontend and a Laravel backend. Both are first-party components of the same platform.

The previous API contract (OA-MVP-007) described token-based authentication, requiring every protected request to carry a bearer token in the Authorization header. The token issuance mechanism was not defined in the contract.

The current platform implementation was found to mix Sanctum stateful SPA authentication (enabled via `statefulApi()` in the backend) with bearer tokens stored in `localStorage` on the frontend. This created two half-configured authentication modes operating simultaneously, with the `localStorage` token approach introducing XSS exfiltration risk.

OA-REV-003 Finding F-5 identified this as architectural drift requiring resolution before VS-002 could begin. OA-REV-003 Finding F-7 identified that the response conventions for authentication endpoints did not align with the approved API contract error model.

A single, consistent authentication transport strategy is required before VS-002.

---

## Decision

Laravel Sanctum SPA cookie-based authentication is the approved authentication strategy for the first-party React SPA in Version 0.1.

The following statements define this decision:

- Authentication uses Sanctum's stateful SPA authentication mechanism.
- Authentication state is maintained through secure HttpOnly session cookies managed by Sanctum.
- The frontend must not store authentication tokens in `localStorage`.
- Protected first-party SPA API requests authenticate through the stateful Sanctum session — no Authorization header with a bearer token is required.
- CSRF protection is part of the authentication flow. State-changing requests (login, logout, and other mutating operations) require a valid CSRF token as mandated by the Sanctum SPA flow.
- The frontend must obtain the Sanctum CSRF cookie before submitting login credentials, as required by the approved Sanctum SPA authentication flow.
- Cross-origin and session configuration must explicitly support the actual frontend origin used by the application.
- The exact environment-specific domains (e.g. stateful domains, session domain, CORS allowed origins) are configuration concerns and must not be hard-coded as architectural requirements.

---

## Identity Scope

The identity model for Version 0.1 is defined by ADR-0005 (OA-ADR-0005). This ADR does not modify the identity model.

- Version 0.1 has exactly one authenticated identity type: **Learner**.
- Learners are manually provisioned by the system operator. There is no self-registration flow.

The following remain explicitly deferred and are not part of Version 0.1:

- Register (self-registration)
- Forgot Password
- Password Reset
- Email Verification
- Admin authentication
- Instructor authentication
- RBAC (Role-Based Access Control)
- Permissions

---

## Consequences

- Existing Bearer-token and `localStorage` authentication in the platform implementation must be reconciled during Stage 2 (Implementation Reconciliation). The frontend must be updated to use the Sanctum cookie/session flow and remove `localStorage` token storage.
- Existing mixed authentication configuration in the platform must be removed so that only the stateful SPA authentication mechanism is active.
- Authentication tests must validate the approved cookie/session flow, not token issuance or `localStorage` persistence.
- The API contract (OA-MVP-007) must be updated to reflect stateful SPA authentication for the first-party SPA. Bearer-token requirements for the first-party SPA are superseded.
- No token issuance endpoint is required for the first-party SPA in Version 0.1. If a future approved ADR introduces another client type (e.g. mobile application, third-party API consumer), token-based authentication may be reintroduced under a separate decision.
- ADR-0005 Consequences stated "Token issuance on successful authentication is the only authentication outcome." This statement is superseded for the first-party SPA by this ADR. The SPA authentication outcome is an authenticated Sanctum session, not a bearer token.
- This decision does not authorize Stage 2 (Implementation Reconciliation). Stage 2 requires a separate task.
- This decision does not authorize VS-002 (Module Overview). VS-002 authorization requires F-5 through F-7 from OA-REV-003 to be closed.

---

## Non-Goals

This ADR does NOT introduce:

- OAuth
- JWT (JSON Web Tokens) for the first-party SPA
- Personal Access Tokens for the first-party SPA
- Mobile-client authentication
- Third-party API authentication
- Multi-role authentication
- RBAC (Role-Based Access Control)

---
