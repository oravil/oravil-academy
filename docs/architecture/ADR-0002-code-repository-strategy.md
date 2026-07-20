# ADR-0002 — Code Repository Strategy

> Document ID: OA-ADR-0002
> Status: Approved
> Date: 2026-07-16

---

## Context

The Oravil Academy project requires two distinct categories of content to reach production:

1. **Platform documentation** — governance, curriculum, standards, architecture, specifications, ADRs, and product briefs.
2. **Application source code** — backend services, frontend application, database migration scripts, and infrastructure configuration.

During the early phases of the project, the `oravil-academy` repository focused exclusively on documentation and curriculum. No application source code has been committed. As Sprint 1 approaches, a decision is required about where application source code will live.

---

## Decision

Application source code will live in a **separate repository** named `oravil-academy-platform`.

The `oravil-academy` repository will remain a **documentation and curriculum repository** containing:

- Governance documents (Constitution, Lifecycle, Standards)
- Curriculum (learning paths, modules, lessons, briefs)
- Architecture Decision Records
- Product specifications (MVP documents, briefs)
- Project execution documentation
- Product backlog

The `oravil-academy-platform` repository will contain:

- Backend application code
- Frontend application code
- Database migration scripts
- Infrastructure configuration
- Environment-level tests

---

## Rationale

**Separation of concerns.**

Documentation and source code have fundamentally different lifecycles, reviewers, tooling requirements, and access patterns. Keeping them in the same repository creates noise, increases repository size, and conflates governance with implementation.

**Independent versioning.**

Documentation versions and software release versions are not synchronized. Separating repositories allows each to be versioned independently.

**Clarity of authority.**

The `oravil-academy` repository documents what must be built and why. The `oravil-academy-platform` repository implements those decisions. This chain is unambiguous when the repositories are separated.

**Contributor clarity.**

Future contributors to curriculum need not clone application code. Future contributors to application code need not navigate curriculum directories.

---

## Consequences

- `docs/PROJECT_EXECUTION_GUIDE.md` Section 15 must not imply that `backend/`, `frontend/`, or `database/` directories exist within this repository.
- Any reference to application directory layout belongs in `oravil-academy-platform` documentation.
- Traceability from `oravil-academy-platform` to `oravil-academy` specifications must be maintained through document references and commit messages.

---

## Status

Approved.

No further action required within this repository beyond removing misleading directory references from PROJECT_EXECUTION_GUIDE.md §15.
