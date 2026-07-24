# ADR-0008 — Arabic Learner Presentation

**Document ID:** OA-ADR-0008
**Version:** 1.0.0
**Status:** Accepted
**Date:** 2026-07-24
**Reference:** OA-ADR-0003 Content Language Policy, OA-ADR-0004 Arabic Presentation Layer, OA-GOV-001 Content Lifecycle, OA-MVP-007 MVP API Contracts, OA-BL-020, OA-BL-027, OA-BL-028

---

## Status

Accepted

---

## Context

The Version 0.1 pilot audience is Egyptian. ADR-0003 established English as the canonical authoring, repository, and documentation language. ADR-0004 established Arabic as a derived presentation layer produced from approved English sources and mandated that the pilot is delivered in Arabic.

The platform implementation is currently English-only end to end: lesson content, interface copy, and error presentation are all rendered in English. OA-BL-020 recorded the resulting pre-launch decision point: run the pilot in English as a documented temporary deviation from ADR-0004, or build the Arabic presentation layer first.

This ADR records that decision and defines the boundary between the Arabic learner-facing surface and the English canonical artifacts it is derived from.

---

## Decision

### Arabic-Only Learner Surface

Version 0.1 presents an **Arabic-only** learner-facing experience.

English remains the canonical language for methodology, curriculum, briefs, specifications, rubrics, standards, terminology, code, APIs, tests, and all documentation.

The learner-facing surface is Arabic in full:

- lesson content
- assignments
- the survey, including its questions and rating labels
- navigation
- buttons
- status labels
- validation and error copy

English must NOT appear as ordinary learner-facing UI copy. English appears on the learner surface only as intentionally retained canonical technical terminology.

Arabic lesson content is produced as an Arabic Learning Adaptation of the approved English lesson per ADR-0004 — authored against the English source, neither a literal translation nor a free re-authoring.

### Canonical Terminology

Where a concept carries a canonical English technical term, the learner-facing presentation retains the canonical English term alongside an approved Arabic rendering — for example: معدل التحويل (Conversion Rate). Approved renderings are recorded in the Canonical Terminology Glossary (`docs/references/GLOSSARY.md`, per ADR-0004), the single terminology authority.

### Error Boundary

The API contract (OA-MVP-007) is unchanged and stays English, including the `error.code` / `error.message` envelope.

The frontend renders Arabic copy keyed on `error.code`. The English `error.message` is NEVER shown to a learner — it is diagnostic only. An unrecognized `error.code` falls back to a generic Arabic message, never to the English message.

### Explicitly Not in Version 0.1

Version 0.1 does NOT introduce:

- an i18n library
- a translation framework
- a language switcher
- a `learners.preferred_language` column
- multilingual storage of any kind (no translation columns, no translations table)

Rationale: a single display language means there is no preference to store. These are abstractions for a problem we do not yet have.

---

## Consequences

- OA-BL-020 is RESOLVED by this decision: the Arabic presentation layer is built before pilot launch; no English-pilot deviation is taken.
- OA-BL-027 (`learners.preferred_language`) is superseded and deferred to v0.2: a single learner-facing display language means there is no language preference to store. It is revisited only if pilot data demonstrates a need for learner-selectable language.
- OA-BL-028's pilot approach stands: inline per-lesson glossary tables, no schema change, with the Canonical Terminology Glossary as the terminology authority behind them.
- ADR-0004 is amended: the derived presentation layer is broadened to the Arabic Learning Adaptation model, with a pointer to this ADR.
- OA-GOV-001 (Content Lifecycle) is amended with the Arabic Learning Adaptation artifact type, the one-way derivation rule, and the Arabic UI Copy Review gate for learner-facing interface strings.
- The platform's current English learner-facing rendering does not conform to this ADR. Bringing it into conformance is presentation-layer work governed by the Arabic UI Copy Review gate; it changes no API contract, schema, or business rule. This ADR establishes the governing rule; it does not by itself schedule the conforming work, which is tasked separately.

---

## Non-Goals

This ADR does NOT:

- produce any Arabic content, UI copy, or terminology renderings
- change any API contract, database schema, or application code
- create the Arabic Learning Adaptation Standard or the Canonical Terminology Glossary — separate governance artifacts produced as the next step
- commit any future version to multi-language support; that question is revisited only per OA-BL-027 on pilot evidence
