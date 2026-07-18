# Platform Foundation Review Memo

**Document ID:** OA-REV-003
**Status:** Action Required — Conditional Approval
**Date:** 2026-07-17
**From:** Product Owner (via external audit)
**To:** Chief Architect, Operations Manager
**Subject:** Platform Foundation review (oravil-academy-platform) — foundation quality is GOOD; approval is conditional on four findings

---

## Summary

The foundation is engineering-sound: layered backend skeleton (Domain/Application/Infrastructure/Shared), real quality tooling (Pint, PHPStan level analysis, Pest, ESLint, Prettier, commitlint, husky), Docker Postgres 16 with healthcheck, a working root Makefile, and CI. This is a credible base.

However, the PR under review is NOT what was declared. It contains the full VS-001 Authentication implementation in addition to the six foundation tasks. Approving it as "foundation" would silently approve unreviewed business functionality and set a precedent that the vertical-slice gate can be skipped.

Approval is CONDITIONAL on F-1 through F-4. F-5 through F-7 are required before VS-002 begins.

---

## Findings

### F-1 — Scope violation: VS-001 is already implemented (CRITICAL, process)

**Owner:** Operations Manager

Declared scope: Bootstrap, Backend Foundation, Frontend Foundation, Docker Foundation, Quality Tooling, make bootstrap.
Actual repo content additionally includes: POST /login, POST /logout, GET /me (backend/routes/api.php, app/Http/Controllers/Auth/*), LoginPage, AuthContext, ProtectedRoute, and both backend and frontend auth tests — i.e. the entire VS-001 scope.

Per OA-GUIDE-001 (Development Workflow) and OA-MVP-010, no slice begins before the foundation is approved. The agent bypassed the gate.

**Required action:** Do NOT discard the code. Formally re-scope this review as "Foundation + VS-001" in the PR description, record the process deviation in the sprint log, and restate to the implementation agent that slice gates are hard stops. If this recurs, split PRs become mandatory.

### F-2 — Schema divergence from OA-MVP-006 (HIGH, blocking)

**Owner:** Chief Architect

Approved schema: `learners` (uuid PK, email, display_name, enrolled_at) with no password column; OA-MVP-007 deliberately left token issuance out of contract scope.
Implementation: default Laravel `users` table (bigint id, name, password, email_verified_at, remember_token) plus password_reset_tokens — none of which exist in the approved schema.

Under "documentation is authoritative," code cannot introduce an identity model the schema does not define.

**Required action:** Amend OA-MVP-006 FIRST (docs before code): define the learner identity + credential model — recommended: rename to `learners`, uuid PK, display_name, add password_hash, drop email_verified_at / remember_token / password_reset_tokens (pilot learners are provisioned, not self-registered — Register and Forgot Password are explicitly out of MVP scope, so their supporting tables must not exist). Then align the migration and User→Learner model.

### F-3 — PHP version divergence (HIGH, one-line fix)

**Owner:** Chief Architect

Approved decision: PHP 8.4. composer.json requires ^8.2; CI pins 8.2. Either amend the recorded decision or set composer to ^8.4 and CI to 8.4. Divergence between an approved decision and the lockfile is exactly the drift OA-GUIDE-001 prohibits, regardless of size.

### F-4 — CI runs zero tests (MEDIUM, blocking)

**Owner:** Operations Manager

Backend CI: Pint + PHPStan only — Pest never runs (no Postgres service in the workflow). Frontend CI: ESLint + Prettier + build — Vitest never runs. Tests exist but the quality gate does not execute them; a green CI currently proves style, not behaviour.

**Required action:** Add a postgres service container and `php artisan test` to the backend job; add `pnpm test` to the frontend job. A slice is "done" only when CI proves it.

---

## Required before VS-002 (not blocking this approval)

### F-5 — Auth transport mode is half-and-half (MEDIUM)

bootstrap/app.php enables statefulApi() (Sanctum SPA cookie mode) while the frontend stores bearer tokens in localStorage — two auth modes half-configured, and localStorage tokens are XSS-exfiltratable. Also SANCTUM_STATEFUL_DOMAINS=localhost:3000 while Vite serves 5173. Pick ONE mode (recommended: Sanctum SPA cookies for a first-party SPA; drop localStorage) and record it as a one-paragraph ADR note.

### F-6 — Declared stack partially fictional (LOW)

TanStack Query and Zod are in package.json but unused (raw fetch; RHF-only validation). Either use them in VS-001 as the approved stack intends (Zod schema for the login form, TanStack Query for /me) or remove them until needed. Declared-but-unused dependencies mislead every future contributor about house patterns.

### F-7 — Response conventions need one alignment pass (LOW)

Invalid credentials return 422 with an ad-hoc {message} body; OA-MVP-007 mandates a consistent response envelope and 401 semantics for authentication failures. Align login/logout/me responses with the contract envelope now, before VS-002 multiplies the endpoint count. Also remove headless-API leftovers: welcome.blade.php, backend resources/js, backend vite.config.js.

---

## Acceptance

Foundation + VS-001 are APPROVED when: the PR is re-scoped per F-1, OA-MVP-006 is amended and the migration aligned per F-2, PHP versions match the recorded decision per F-3, and CI executes both test suites per F-4 — all committed referencing OA-REV-003.

VS-002 (Module Overview) is authorized only after F-5 through F-7 are closed. VS-002 is the first slice that touches the approved domain schema; it must not start on top of an unreconciled identity model.
