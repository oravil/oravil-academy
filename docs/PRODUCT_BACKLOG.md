# PRODUCT BACKLOG

> Document ID: OA-BACKLOG-001
> Status: Active

---

## Rules

Only items that have been reviewed and intentionally deferred belong here.

Backlog items are NOT part of the current release.

Items move from Backlog to Active Planning only after formal approval.

---

# Release v1

## Deferred Improvements

### OA-BL-001

Title:
Move Campaign Objectives before platform-specific advertising modules.

Reason:
Improve learning progression.

Priority:
Medium

Status:
Deferred

---

### OA-BL-002

Title:
Move KPI Framework before GA4.

Reason:
Introduce measurement concepts before analytics tools.

Priority:
Medium

Status:
Deferred

---

### OA-BL-003

Title:
Add Introduction to Marketing Measurement in Phase 0.

Reason:
Provide early exposure to marketing measurement concepts.

Priority:
High

Status:
Deferred

---

### OA-BL-004

Title:
Introduce UTM Tracking fundamentals.

Reason:
Essential for campaign attribution.

Priority:
High

Status:
Deferred

---

### OA-BL-005

Title:
Add Landing Page Fundamentals.

Reason:
Critical connection between traffic acquisition and conversion.

Priority:
High

Status:
Deferred

---

### OA-BL-006

Title:
Introduce Operational QA before campaign launch.

Reason:
Reflect real agency workflows.

Priority:
Medium

Status:
Deferred

---

### OA-BL-007

Title:
Embed Business Simulation into lesson production.

Reason:
Prepare learners for real-world execution.

Priority:
High

Status:
Deferred

---

### OA-BL-008

Title:
Introduce Review and Revision workflow.

Reason:
Reflect professional agency collaboration.

Priority:
High

Status:
Deferred

---

### OA-BL-009

Title:
Author and publish CONTENT_STANDARD.md (OA-STD-002).

Reason:
CONTENT_STANDARD.md was removed as a placeholder. A formal content standard governing tone, structure, and voice is required before content production scales beyond Module 1.

Priority:
High

Status:
Deferred

---

### OA-BL-010

Title:
Author and publish MARKDOWN_STANDARD.md (OA-STD-003).

Reason:
A formal Markdown standard governing formatting conventions, heading levels, and file structure is required for consistent document authoring across the repository.

Priority:
Medium

Status:
Deferred

---

### OA-BL-011

Title:
Author and publish NAMING_STANDARD.md (OA-STD-004).

Reason:
A formal naming standard governing file names, identifiers, and directory names is required before the codebase and curriculum expand significantly.

Priority:
Medium

Status:
Deferred

---

### OA-BL-012

Title:
Author and publish QUIZ_STANDARD.md (OA-STD-005).

Reason:
A quiz standard is required before quiz production begins in Phase 5.

Priority:
Medium

Status:
Deferred

---

### OA-BL-013

Title:
Author and publish LAB_STANDARD.md (OA-STD-006).

Reason:
A lab standard is required before lab production begins in Phase 5.

Priority:
Medium

Status:
Deferred

---

### OA-BL-014

Title:
Author and publish CASE_STUDY_STANDARD.md (OA-STD-007).

Reason:
A case study standard is required before case study production begins in Phase 5.

Priority:
Medium

Status:
Deferred

---

### OA-BL-015

Title:
Author and publish SOP_STANDARD.md (OA-STD-008).

Reason:
A standard operating procedure standard is required before SOP-based lessons are introduced.

Priority:
Low

Status:
Deferred

---

### OA-BL-016

Title:
Author and publish DESIGN_SYSTEM.md (OA-STD-009).

Reason:
A design system standard is required before UI development begins on the platform frontend.

Priority:
Medium

Status:
Deferred

---

### OA-BL-017

Title:
v0.2 candidate: AI-assisted content generation pipeline.

Reason:
Admin-facing flow where an approved Module Brief generates draft Lesson Briefs, and approved Lesson Briefs generate draft lessons conforming to LESSON_STANDARD.md's 14-section structure, with web search for current sources. Drafts pass through the existing CONTENT_LIFECYCLE quality gates unchanged (Technical Review, Educational Review, Final Approval remain human; per CONTENT_LIFECYCLE §5, an AI agent may not approve its own output). Gated on v0.1 methodology validation per OA-MVP-001 Definition of Success.

Priority:
Low

Status:
Deferred

---

### OA-BL-018

Title:
v0.2 candidate: second learning path (Management).

Reason:
Validates the path-agnostic methodology claim; requires path-selection UI (out of v0.1 scope) and confirms the sequential-progression rule fits non-marketing content (may need an ADR if branching/optional structures are required). Zero schema changes needed — learning_paths table already supports multiple rows. Gated on v0.1 validation.

Priority:
Low

Status:
Deferred

---

### OA-BL-019

Title:
Add phase/module position fields to the Module Overview response contract (OA-MVP-007).

Reason:
MVP_WIREFRAMES.md Screen 1 specifies the module heading as "Phase 0 — Module 1: {title}",
but GET /v1/modules/{module_id}/overview (OA-MVP-007) returns only a bare `title` — no
phase number or module position — so the frontend cannot compose that heading without
inventing data not present in the response. Surfaced implementing VS-002 Phase C
(OA-HANDOFF-001 Task 7); the frontend renders the bare title in the interim. Requires
a contract change to OA-MVP-007, so it was not implemented as part of that slice —
should ride along when a future slice next touches this contract.

Priority:
Low

Status:
Deferred

---

### OA-BL-020

Title:
Decision required before pilot launch: Arabic pilot language vs. English-only v0.1.

Reason:
ADR-0004 mandates the pilot runs in Arabic as a derived presentation layer, but the
current platform pipeline (schema, seeder, frontend) is English-only with no
translation mechanism — no translation columns, no translations table, no locale
switching. Product Owner must decide before launch: (a) launch the first pilot in
English as a documented temporary deviation from ADR-0004, or (b) build the Arabic
presentation layer first (schema + seeder + frontend work, scope TBD). Blocks pilot
launch scheduling.

Priority:
High

Status:
Deferred

---

### OA-BL-021

Title:
Pre-pilot visual identity baseline pass across the five MVP screens.

Reason:
Before the pilot, apply a minimal visual identity (Oravil brand colors, readable
long-form typography, spacing appropriate for educational content) across the five
MVP screens. Explicitly NOT the full dynamic/interactive design vision — this is the
minimum polish needed so that visual impressions don't contaminate the
methodology-satisfaction signal in the post-module survey (OA-MVP-001 success
criterion 4: 80% rating 4+/5). Scope: styling only, no new features, no
gamification.

Priority:
Medium

Status:
Deferred

---

### OA-BL-022

Title:
v0.2 candidate: interactive learning experience.

Reason:
Dynamic, engaging lesson presentation (interactions, animations, interactive
knowledge checks with feedback, progress visualizations). Sits alongside the
existing v0.2 candidates (OA-BL-017 AI-assisted content generation, OA-BL-018
second learning path). Gated on v0.1 methodology validation; the automated
quizzes/assessments portion also maps to ROADMAP Phase 5 (Practical Assets).

Priority:
Low

Status:
Deferred

---

### OA-BL-023

Title:
Add a reading-progress indicator to the Lesson View screen.

Reason:
MVP_WIREFRAMES.md Screen 2 (Lesson View) specifies a reading progress
indicator; it is documented as informational-only and was not one of the
six requirements explicitly declared for VS-003 Phase C (OA-HANDOFF-001
Task 7), so it was deliberately left out of that slice. Purely cosmetic —
no contract or schema change needed.

Priority:
Low

Status:
Deferred

---

### OA-BL-024

Title:
Server memory margin: dev box runs at ~1.9GB RAM with a history of OOM
kills during dependency installs.

Reason:
Installing react-markdown + remark-gfm for VS-003 (OA-HANDOFF-001 Task 7
Phase C) OOM-killed `pnpm add` under normal conditions (available memory
regularly under 200MB with zero swap configured). Worked around with a
temporary 2GB swapfile for the duration of the install, then torn down —
not a permanent fix.

The gap between teardown and this resolution proved the risk concrete:
on 2026-07-22, with swap back at 0B, a genuine OOM storm (system-wide,
window ~19:55-20:11) killed `mysqld` fourteen times in a restart-crash
loop and terminated the `make dev` foreground process (`artisan serve`
+ `vite`), interrupting the running dev stack mid-session.

Resolution (2026-07-22): (1) `mysql.service` was found enabled with zero
consumers in this stack (`DB_CONNECTION=pgsql` throughout; the `mysql`
block in `config/database.php` is unused Laravel boilerplate; nothing in
`.env` references it) — stopped and disabled
(`systemctl stop/disable mysql`), removing its restart-crash-loop as a
future memory hazard entirely. (2) A 2GB swapfile was recreated at
`/swapfile`, this time added to `/etc/fstab` so it persists across
reboots, rather than the prior session's temporary one.

Priority:
Medium

Status:
Resolved (2026-07-22)

---

### OA-BL-026

Title:
Pilot provisioning tooling (pre-launch, required): artisan commands for
learner lifecycle management.

Reason:
ADR-0005 mandates manual learner provisioning with no self-registration,
but no document defines the mechanism. Product Owner decision (2026-07-23):
artisan CLI commands, not an admin panel — (a) create learner (email,
display name, password), (b) reset learner password, (c) export
submissions for manual rubric grading. Chosen over an admin panel for
v0.1: 10 learners, one-week window, single grader. Admin panel with a
non-Learner identity remains a v0.2 candidate and is the natural first
feature there, since public enrolment is a stated future direction.
Resolves the provisioning-mechanism open question logged in
OA-HANDOFF-001 §5.

Priority:
High

Status:
Deferred (required before pilot launch)

---

### OA-BL-027

Title:
learners.preferred_language column — amends ADR-0005.

Reason:
Product Owner decision (2026-07-23): add a preferred_language column to
learners, amending ADR-0005's five-column identity model. Requires the
ADR-0005 amendment (docs first) plus a migration and an OA-MVP-006
update — documentation precedes code per ADR-0002. Belongs to the i18n
slice that follows Step 9 of OA-MVP-010 (content translation storage,
UI string extraction, RTL support, language switcher), not to VS-006.
Do not implement until that slice begins.

Priority:
Medium

Status:
Deferred

---

### OA-BL-028

Title:
v0.2 candidate — central terminology database (glossary entities).

Reason:
Product Owner decision (2026-07-23): for the pilot, each translated
lesson carries its own inline glossary table written directly into the
lesson content (content work, no schema change) — technical terms stay
in English in both cases (market-standard vocabulary), explained in the
glossary. A v0.2 candidate is distinct and richer: glossary entities
linked to lessons, cross-referenced and browsable as a standing
reference across the platform, rather than duplicated per lesson.
Gated on v0.1 validation, alongside the i18n slice.

Priority:
Low

Status:
Deferred

---

### OA-BL-025

Title:
v0.2 candidate — AI-assisted submission grading with tiered approval.

Reason:
After a learner submits (VS-004 flow), an AI evaluation job scores the
submission against the Lesson Brief's rubric and produces structured
output (per-criterion scores, learner-facing feedback, reviewer-facing
notes, confidence score). A reviewer/observer screen presents submissions
alongside AI evaluations for approve/amend/reject. Approval mode is
configurable per assignment or per course, three tiers: (1)
human-approved — nothing reaches the learner without reviewer sign-off;
(2) AI-auto with oversight — published automatically, full audit log,
retroactive intervention; (3) no approval — direct publish for low-stakes
exercises. Tier-1 operation generates calibration data (reviewer
amendments vs AI output) that gates promotion to tier 2 per criterion,
evidence-based. Requires: submission status expansion via migration
(evaluated/approved states — CHECK constraint change, OA-MVP-006
amendment first), evaluations + feedback tables, reviewer role (first
non-Learner identity — needs an ADR amending ADR-0005), reviewer UI,
learner feedback display in Lesson View. Gated on v0.1 validation AND on
manual grading of the pilot cohort's submissions, which produces the
rubric-calibration baseline this system needs. Sits alongside the
AI-assisted content generation candidate — together they form the "fully
intelligent academy" direction.

Priority:
Medium

Status:
Deferred
