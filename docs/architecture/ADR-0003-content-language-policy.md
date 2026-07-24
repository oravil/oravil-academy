# ADR-0003 — Content Language Policy

> Document ID: OA-ADR-0003
> Status: Approved
> Date: 2025-07-16
> Superseded: Partially by ADR-0004 (sections: MVP Pilot Language, First Localization Target); further by ADR-0008

---

## Context

Oravil Academy is designed to serve a professional audience in Arabic-speaking markets. Arabic is the primary target audience language. At the same time, the majority of the source materials, technical documentation, and industry-standard terminology exist in English.

Before content production proceeds at scale, a formal decision is required on:

1. Which language is used for content authoring.
2. How localization is handled.
3. What language the MVP pilot runs in.
4. What language governs repository documentation.

---

## Decision

### Canonical Authoring Language

**English** is the canonical authoring language for all content produced within this repository.

All lesson content, course narratives, module briefs, and lesson briefs are authored in English first.

### Repository Language

All governance documents, standards, ADRs, product briefs, and technical documentation are written in English.

### MVP Pilot Language

The Version 0.1 MVP pilot will run in **English only**.

No localized content will be produced before the MVP methodology validation is complete.

### First Localization Target

**Arabic** is the first localization target after MVP validation.

Localization will be handled as a post-MVP capability. It is recorded in the Product Backlog and will require a defined localization workflow before production begins.

---

## Rationale

**Single source of truth.**

Authoring in one language first prevents divergence between versions. The English source is the canonical record. Translations are derived from it.

**MVP scope discipline.**

Producing content in two languages simultaneously would double content production effort without producing additional methodology signal. Localization belongs after validation.

**Industry standard practice.**

English is the global standard for technical and business education documentation. Authoring in English first preserves access to the widest range of reviewers, tooling, and reference materials.

**Arabic-first market strategy is preserved.**

The decision to treat Arabic as the first localization target ensures the long-term audience strategy is recorded and not forgotten during the English-first production phase.

---

## Consequences

- All lesson content files in `academy/` are authored in English.
- Localization directories or translated content must not be created before a formal localization workflow is approved.
- When localization begins, a new ADR must be written to define the translation process, file structure, and review workflow.
- The Product Backlog must carry an item for the Arabic localization capability.

---

## Status

Approved.
