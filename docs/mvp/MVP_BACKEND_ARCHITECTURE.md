# MVP Backend Architecture

**Document ID:** OA-MVP-008
**Version:** 1.0.0
**Status:** Approved
**Date:** 2026-07-16
**Reference:** OA-MVP-001 MVP Product Brief, OA-MVP-002 MVP Information Architecture, OA-MVP-003 MVP User Flows, OA-MVP-004 MVP Wireframes, OA-MVP-005 MVP Domain Model, OA-MVP-006 MVP Database Schema, OA-MVP-007 MVP API Contracts

---

## Purpose

This document defines the backend architecture for Version 0.1 of Oravil Academy. It describes the layered structure, request lifecycle, module organization, cross-cutting concerns, and the constraints that govern how the system is built. It does not specify technology, framework, infrastructure, or deployment.

---

## Architectural Style

Version 0.1 uses a layered architecture organized around the domain model. The layers are API, Application, Domain, Infrastructure, and Persistence. Each layer has a single, bounded responsibility and depends only on layers below it. Business rules live exclusively in the Domain layer and are never duplicated elsewhere.

This style is appropriate for Version 0.1 because the product is small and well-understood, the domain model is stable, and the primary risk is not scale but correctness. A layered architecture provides the clarity needed to validate the learning methodology without over-engineering the codebase. It also creates a natural seam for testing each layer in isolation, which is essential when the product is being evaluated against real learner behaviour.

---

## Design Principles

1. Domain-first — the Domain layer defines all business rules; no other layer is permitted to contain rule logic.
2. Thin controllers — the API layer translates HTTP requests into application commands and application results into HTTP responses; it contains no logic beyond this translation.
3. Application services as orchestrators — the Application layer coordinates domain objects and infrastructure calls in a defined sequence; it owns workflow but not rules.
4. Explicit validation at the boundary — all request validation occurs at the API layer before the request reaches the Application layer; invalid requests never enter the system.
5. Dependency inversion — the Domain and Application layers depend on abstractions, not on concrete infrastructure or persistence implementations.
6. Stateless services — no service holds state between requests; all state is sourced from the Persistence layer on each request.
7. Predictable composition — each request maps to exactly one application service method; there is no implicit routing of logic through multiple entry points.

---

## High-Level Layers

### API Layer

Receives HTTP requests, authenticates the caller, validates the request payload against the rules defined in OA-MVP-007, maps valid requests to application service calls, and maps application results to HTTP responses. The API layer has no knowledge of business rules. It knows only the shapes defined in the API contracts.

### Application Layer

Contains application services — one per feature area. Each service method represents a single use case (for example: submit assignment, get module progress, submit survey). The Application layer orchestrates calls to the Domain layer and the Persistence layer in the correct sequence to fulfil the use case. It does not enforce business rules directly; it delegates to Domain objects. It does not execute queries directly; it delegates to Persistence interfaces.

### Domain Layer

Contains the domain entities, value objects, and business rules defined in OA-MVP-005. This is the authoritative layer for all rule enforcement — submission lifecycle transitions, lesson unlock logic, module completion determination, and survey access conditions are all enforced here. The Domain layer has no dependencies on any other layer. It does not know how data is stored, how requests arrive, or how responses are formatted.

### Infrastructure Layer

Contains concrete implementations of interfaces defined in the Application and Domain layers. This includes authentication token verification, external integrations if any are required, and any cross-cutting technical concerns. The Infrastructure layer adapts the outside world to the shapes the inner layers expect.

### Persistence Layer

Contains the data access logic — queries, inserts, and updates against the database schema defined in OA-MVP-006. Persistence is accessed only through interfaces defined in the Application layer, ensuring the Domain and Application layers remain independent of storage technology. The Persistence layer contains no business logic and no workflow logic.

---

## Request Lifecycle

```
HTTP Request
    ↓
API Layer — Authentication
    ↓
API Layer — Request Validation
    ↓
API Layer — Map to Application Command
    ↓
Application Service — Orchestrate Use Case
    ↓
Domain — Enforce Business Rules
    ↓
Persistence — Read or Write Data
    ↓
Application Service — Compose Result
    ↓
API Layer — Map to HTTP Response
    ↓
HTTP Response
```

If authentication fails, the request is rejected at the Authentication step with a 401 response. If validation fails, the request is rejected at the Request Validation step with a 422 response. If a domain rule is violated — for example a learner attempts to access a locked lesson — the Domain layer signals the failure to the Application layer, which propagates it to the API layer as a 403 response. No step is skipped and no layer is bypassed.

---

## Module Organization

The backend is organized by feature domain rather than by technical role. Each feature domain maps to a bounded area of the product described in OA-MVP-005.

**Learner** — manages learner identity, authentication context, and the learner's relationship to enrolled content.

**Learning Path** — manages the read-only content hierarchy: learning paths, phases, modules, and lessons. This area is responsible for serving structured content to the API layer on demand.

**Progress** — manages the derived progress state for a learner within a module. Progress is computed from assignment submission events and survey completion events; this area owns the logic for deriving and returning that state.

**Assignment** — manages the assignment submission lifecycle: validating a submission request, applying domain rules, persisting the submission, and triggering the downstream effects (lesson unlock or module completion).

**Survey** — manages survey retrieval and survey response submission: enforcing access conditions, persisting answers, and marking the survey as submitted for the learner.

Each feature domain contains its own API handlers, application service, domain logic relevant to that area, and persistence interface. Cross-domain dependencies are explicit and directional — for example, the Assignment domain may query the Learning Path domain to determine lesson order, but never the reverse.

---

## Cross-Cutting Concerns

### Authentication

Every request passes through a single authentication step at the API layer boundary. The authentication mechanism verifies the bearer token and resolves the authenticated learner identity. This identity is passed explicitly to every application service call. No service resolves the caller identity independently.

### Authorization

Authorization is enforced at the Domain layer, not the API layer. The API layer establishes who the caller is; the Domain layer determines what they are permitted to do based on their current progress state. Examples: a learner may not access a locked lesson; a learner may not submit a survey response before completing the module; a learner may not submit an assignment twice.

### Validation

Request validation is enforced at the API layer using the rules defined in OA-MVP-007. Validation failures return a 422 response with a field-level error list as defined in the error model. No invalid request reaches the Application layer.

### Logging

All inbound requests are logged at the API layer with the request method, route, and response status code. Application service entry and exit points are logged with the use case name and outcome. Domain rule violations are logged as warnings. Errors that result in 500 responses are logged with full context for diagnosis.

### Error Handling

Unhandled errors are caught at the API layer boundary and returned as a 500 response using the standard error model defined in OA-MVP-007. Domain rule violations and access control failures are signalled as typed results from the Domain and Application layers and translated to the appropriate HTTP status code at the API layer. Raw exception details are never exposed in responses.

### Configuration

Environment-specific configuration — database connection, token secret, and any external service credentials — is sourced from the environment at startup and injected into the Infrastructure and Persistence layers. Configuration is never hard-coded and never accessed directly from the Domain or Application layers.

---

## Architectural Constraints

1. The Domain layer must not depend on the Infrastructure, Persistence, or API layers.
2. The Application layer must not depend on the Infrastructure or API layers directly.
3. The API layer must not contain business rules or workflow logic.
4. The Persistence layer must not contain business rules or workflow logic.
5. Controllers must not call Persistence interfaces directly; they must go through Application services.
6. Domain rules must not be duplicated in the Application or API layers.
7. All request validation must occur at the API layer boundary before the Application layer is invoked.
8. Authorization decisions must be made by the Domain layer, not the API layer.
9. Every application service method must correspond to exactly one use case.
10. No service may hold mutable state between requests.
11. All dependencies between feature domains must be explicit and unidirectional.
12. The authentication context must be passed explicitly to every application service call; it must not be read from a global or ambient context.
13. Error responses must always conform to the error model defined in OA-MVP-007; no endpoint may return an error in a different shape.
14. Raw database errors must never propagate to the API response; they must be caught at the Persistence layer boundary and converted to typed results.
15. Configuration values must never be accessed directly from the Domain or Application layers.

---

## Use Case Ownership

Every public use case has exactly one owning feature domain. That domain is solely responsible for business correctness for that use case. Coordination with other domains is permitted but ownership is never shared.

| Use Case | Owning Feature Domain | Notes |
|---|---|---|
| Get Module Overview | Learning Path | Delegates to Progress for per-learner lesson status |
| Get Lesson | Learning Path | Delegates to Progress to enforce locked/available/complete state |
| Submit Assignment | Assignment | Coordinates with Progress to trigger lesson unlock or module completion |
| Get Progress | Progress | Derives state from Assignment and Survey submission events |
| Get Module Completion | Progress | Enforces completion precondition; delegates to Learning Path for lesson list |
| Get Survey | Survey | Enforces module-complete precondition via Progress; delegates to Learning Path for module context |
| Submit Survey | Survey | Coordinates with Progress to confirm module is complete before accepting submission |

---

## Future Expansion

The architecture is designed to grow by addition, not by modification. Adding a new learning path or module requires no changes to the existing layer structure — new content is added to the database and served by the existing Learning Path and Progress feature domains. Adding a new role — for example an instructor or an administrator — requires adding a new authentication context type and new feature domains scoped to that role's use cases; no existing domain logic is modified. Adding new API capabilities, such as enrolment management or certificate issuance, requires adding new feature domains alongside existing ones under the same layered structure. The constraint that the Domain layer has no outward dependencies means it can be tested and extended without touching infrastructure, making the cost of adding behaviour low and the risk of regression contained.
