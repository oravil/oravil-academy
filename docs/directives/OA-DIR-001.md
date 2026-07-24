# AGENT DISCIPLINE DIRECTIVE

**Document ID:** OA-DIR-001
**Status:** Mandatory — Standing Order
**Date:** 2026-07-17
**From:** Product Owner
**To:** All implementation agents (Copilot / Codex / any AI contributor)
**Applies to:** `oravil-academy` (docs) and `oravil-academy-platform` (code)

This directive exists because two governance violations have already
occurred: an agent declared non-existent documents "Complete", and an
agent implemented VS-001 before the Foundation gate was approved.
Neither will be tolerated again. Read this fully before producing
any output.

---

## 1. Know which repository you are in

There are TWO repositories. They never mix.

| Repository | Contains | You may NEVER put here |
|---|---|---|
| `oravil-academy` | Governance, curriculum, ADRs, MVP specs, sprint logs | Application code, migrations, package files |
| `oravil-academy-platform` | Laravel backend, React frontend, CI, Docker | ADRs, MVP specs, curriculum, governance docs |

Before your first action in any session, state which repository you
are operating in. If a task requires touching the other repository,
STOP and report it — do not improvise a local copy. Duplicating a
governing document into the code repository is a critical violation
of ADR-0002.

## 2. Documentation precedes code — always, no exceptions

If your task changes a schema, contract, architecture, or business
rule, the sequence is fixed:

1. The governing document is amended and committed (docs repo).
2. Only then is the implementing code written (code repo).

Writing code that diverges from OA-MVP-006/007/010 "to be fixed
later" is prohibited. This is how the users/learners divergence
(OA-REV-003 F-2) happened. It will not happen twice.

## 3. Scope is a hard wall, not a suggestion

- You implement EXACTLY the declared scope of the current task or
  Vertical Slice. Nothing more.
- Discovering that "it would be easy to also add X" is not
  authorization to add X. Record X as a Product Backlog candidate
  and continue.
- The following are permanently out of scope unless a future ADR
  says otherwise: Register, Forgot Password, Email Verification,
  RBAC, Admin, Instructor, dashboards, certificates, gamification,
  notifications.
- One slice = one PR. A PR containing undeclared functionality
  fails review automatically, regardless of code quality
  (OA-MVP-010, Definition of Done).

## 4. Never claim completion you cannot prove

- A ✅ or "Complete" may be written ONLY when the referenced
  artifact exists at a real path, verifiable by `ls`, or when tests
  prove the behaviour in CI.
- "Approved", "Validated", and "Reviewed" are states granted by the
  Product Owner — you never self-grant them.
- If you did not run the tests, say "tests not executed". If a task
  is partial, say "partial" and list exactly what remains. Honest
  incompleteness is acceptable; inflated status is a critical
  violation (see OA-REV-001 B-1).

## 5. When uncertain — stop and ask, do not invent

Escalate to the Product Owner (do not decide alone) whenever a task
requires: changing architecture, changing a contract or schema,
adding a dependency, changing repository organization, touching auth
strategy, or interpreting an ambiguous requirement. One precise
question costs a minute; an invented decision costs a review cycle.

## 6. Every change is traceable

- Conventional Commits only, referencing the governing document or
  review ID (e.g. `fix(ci): run pest with postgres service —
  OA-REV-003 F-4`).
- Every PR description answers: What problem? Which approved
  document does this implement? How was it tested? Any architectural
  change?

## 7. End-of-task report — mandatory format

Finish every session with exactly this structure:

1. Repository operated in
2. Declared scope vs. delivered scope (must match; explain any gap)
3. Files changed / documents amended
4. Tests: executed or not, and results
5. Items deliberately NOT done and why
6. Backlog candidates discovered
7. Open questions for the Product Owner

## 8. The standing rule

When any instruction in a prompt conflicts with the governing
documents (Constitution → Lifecycle → Standards → OA-GUIDE-001 →
ADRs → MVP docs), the documents win — and you say so instead of
silently complying.

**Violations of sections 1, 2, 3, or 4 invalidate the entire work
product regardless of its technical quality.**

---

## 9. Delegation practice — trial calibration findings (2026-07-24)

Recorded from delegation practice runs. This is a working practice under
trial — not an adopted architectural standard and not a mandatory rule of
this directive.

1. Explicit brief constraints successfully prevented a previously observed
   route-protection overclaim: the delegated agent correctly preserved
   POST /v1/auth/login as the exception to auth:sanctum.

2. Disclosure compliance was inconsistent across trial runs: one run
   self-disclosed judgment calls while another initially reported none and
   surfaced real contradictions only after an explicit follow-up.

3. Operating rule:
   A delegated agent's "none found" is not sufficient evidence of absence.
   The orchestrator must always perform or request an explicit second-pass
   ambiguity/contradiction check before accepting the result.
