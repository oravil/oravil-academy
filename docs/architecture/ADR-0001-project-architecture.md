# ADR-0001 — Project Architecture

**Document ID:** OA-ADR-0001
**Status:** Accepted
**Date:** 2026-07-16
**Author:** Chief Architect, Oravil Academy

---

## Context

Oravil Academy is building a learning platform from scratch. The primary risk at the start of the project was not technical complexity — the Version 0.1 feature set is intentionally small — but architectural drift: making implementation decisions before the product was fully defined, building infrastructure before validating the methodology, and allowing business rules to scatter across layers as the system grew.

A second risk was discontinuity. Multiple contributors — content producers, product architects, software architects, frontend and backend engineers — need to work from a single, coherent specification without contradicting each other. Without explicit architectural decisions recorded before implementation, each contributor would make local decisions that seemed reasonable in isolation but produced an inconsistent system.

The project therefore required explicit architectural principles to be documented and approved before any line of implementation code was written. These principles needed to span the product definition, the domain model, the data layer, the API surface, and both the backend and frontend application layers.

---

## Decision

The following architectural decisions were adopted for Version 0.1 and are reflected across the approved MVP documents OA-MVP-001 through OA-MVP-010.

**Product-first development.** No implementation began until the product was fully defined. The MVP Product Brief, Information Architecture, User Flows, and Wireframes were completed and approved before any technical specification was written. This ensures implementation serves a validated product definition, not a speculative one.

**Domain-first design.** The Domain Model (OA-MVP-005) was defined before the database schema, API contracts, or application architecture. All downstream technical documents are derived from and constrained by the domain model. Business rules live in the Domain layer and are not duplicated in any other layer.

**Vertical slice implementation.** Development proceeds one complete feature at a time, from database to UI. No horizontal layer is built ahead of the feature that requires it. This ensures working software exists at every step and that integration problems are discovered immediately rather than at the end of the project.

**Feature-oriented architecture.** Both the backend and frontend are organized by feature domain — Learner, Learning Path, Progress, Assignment, Survey — rather than by technical role. This co-locates related code and makes each feature independently understandable, testable, and modifiable.

**Server as source of truth.** Learner progress, lesson access state, submission status, and survey completion are always sourced from the server. The frontend never derives, assumes, or optimistically updates these values. This eliminates an entire class of consistency errors in a sequentially gated product where incorrect state presentation would corrupt the learner's experience and invalidate the methodology data being collected.

**API contract before implementation.** The API contracts (OA-MVP-007) were written and approved before either the backend or the frontend began implementation. The contracts define the interface boundary; neither side may assume what the other will provide beyond what is specified there. This allows backend and frontend development to proceed in parallel without drift.

**Documentation as specification.** Architecture documents are the specification. Implementation decisions that deviate from an approved document require the document to be updated first. This creates a single source of truth for every decision and prevents the codebase from diverging silently from its intended design.

**Sequential learner progression.** The product enforces a strict lesson sequence. A learner cannot access a lesson until the previous one is complete. This rule is enforced at three levels: the domain model, the API layer, and the frontend navigation layer. No layer trusts another to enforce the rule on its behalf.

---

## Consequences

**Positive consequences:**

- Every implementation decision has a traceable specification. Developers do not make architectural choices independently.
- Business rules are isolated in the Domain layer. They can be tested, reviewed, and changed without touching controllers, database queries, or UI components.
- The API contract is stable before development begins, enabling parallel backend and frontend work.
- The product is defined before implementation, reducing the risk of building something that does not validate the intended methodology.
- The system can accommodate additional modules and learning paths without architectural change.

**Trade-offs accepted:**

- Significant time was invested in documentation before a single line of implementation code was written. This is the deliberate cost of clarity.
- The architecture is more structured than a project of this size strictly requires. This overhead is accepted because the product is expected to grow significantly after the methodology is validated.
- Optimistic updates and client-side progress caching were rejected in favour of server-authoritative state. This means slightly more API calls and a dependency on network availability, both of which are acceptable constraints for a methodology-validation product.

**Things intentionally not optimized:**

- Performance at scale. Version 0.1 serves a small pilot group. No caching layer, no CDN strategy, and no query optimization beyond basic indexing is in scope.
- Developer experience tooling. The architecture is defined in a technology-independent way. Framework selection, build tooling, and developer workflow are implementation decisions deferred to the development team.
- Feature richness. Certificates, gamification, community features, and AI-assisted feedback are explicitly out of scope for Version 0.1.

---

## Alternatives Considered

**Big-bang development.** Building all features simultaneously before integration testing any of them. Rejected because it defers all integration risk to the end of the project and produces no demonstrable working software until everything is complete. The vertical slice approach was adopted instead.

**Horizontal layers first.** Building the full database schema, then the full API layer, then the full frontend layer as sequential phases. Rejected because it delays end-to-end integration and makes it difficult to validate that the architecture works as intended until all three layers are connected. Vertical slices produce a working, integrated increment at each step.

**Code-first design.** Beginning implementation and letting the architecture emerge from the code. Rejected because it produces a codebase where architectural decisions are implicit, inconsistent, and difficult to change. Documenting decisions before implementation makes them explicit, reviewable, and enforceable.

**API-last design.** Building backend and frontend independently and defining the API contract from what each side produced. Rejected because it guarantees contract divergence and rework. The API contract was defined first and both sides were built to it.

---

## Related Documents

- OA-MVP-001 — MVP Product Brief
- OA-MVP-002 — MVP Information Architecture
- OA-MVP-003 — MVP User Flows
- OA-MVP-004 — MVP Wireframes
- OA-MVP-005 — MVP Domain Model
- OA-MVP-006 — MVP Database Schema
- OA-MVP-007 — MVP API Contracts
- OA-MVP-008 — MVP Backend Architecture
- OA-MVP-009 — MVP Frontend Architecture
- OA-MVP-010 — MVP Implementation Plan
