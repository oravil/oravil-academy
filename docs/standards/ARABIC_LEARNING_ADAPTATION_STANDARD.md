# ARABIC LEARNING ADAPTATION STANDARD

> **Document ID:** OA-STD-002
> **Version:** 1.0.0
> **Status:** Active
> **Owner:** Oravil Academy
> **Applies To:** All Arabic learner-facing content produced for Oravil Academy v0.1

---

# 1. Purpose

This document defines the mandatory standard for producing Arabic learner-facing content for Version 0.1: the Arabic Learning Adaptation of approved English lessons, and Arabic learner-facing UI copy.

It records Product-Owner-approved decisions. It does not reopen them.

Governing authorities: ADR-0003 (Content Language Policy), ADR-0004 (Arabic Presentation Layer, as amended), ADR-0008 (Arabic Learner Presentation), and OA-GOV-001 Content Lifecycle §9.

---

# 2. Language Model

English remains canonical for:

- governance
- methodology
- curriculum structure
- specifications
- learning objectives
- approved English learning content
- briefs
- rubrics
- standards
- technical terminology
- developer-facing material
- code
- APIs

Arabic is the learner-facing language for v0.1.

The Arabic learner artifact is called an **Arabic Learning Adaptation**.

It is NOT:

- a literal translation;
- an independent canonical curriculum;
- free authorship detached from the approved English source.

---

# 3. Source / Derivation Rule

An Arabic Learning Adaptation MUST be anchored to the approved canonical English learning content itself, not merely to its brief.

The adaptation may naturally rewrite:

- explanations
- sentence structure
- examples
- cultural/contextual framing

when doing so improves comprehension for an Arabic-speaking learner.

It MUST preserve:

- learning objectives
- instructional scope
- sequence
- required concepts
- assignment intent
- required deliverable
- rubric/evaluation meaning

The Arabic artifact must teach the same approved lesson naturally in Arabic; it must not become a different lesson.

---

# 4. One-Way Governance

The direction of authority is strictly:

```
Canonical English
      ↓
Arabic Learning Adaptation
```

- If canonical English content changes, the corresponding Arabic adaptation must be marked and reviewed for synchronization (per the ADR-0004 Derivation Rule and Sync Status metadata).
- An Arabic adaptation NEVER silently changes the canonical English source.
- Any proposed curriculum or content improvement discovered while adapting Arabic must be escalated separately against the canonical English source.

---

# 5. Terminology

Canonical technical terminology remains English.

Arabic learner content uses an approved Arabic rendering while preserving the canonical English term where pedagogically useful.

Preferred first-use pattern:

```
Arabic rendering (Canonical English Term)
```

Example:

```
معدل التحويل (Conversion Rate)
```

Abbreviations commonly encountered in real tools may remain visible:

```
نسبة النقر إلى الظهور (Click-Through Rate — CTR)
```

Do not invent multiple Arabic renderings for the same canonical term.

The Canonical Terminology Glossary (`docs/references/GLOSSARY.md`, per ADR-0004) is the single source of truth for approved renderings. No other document serves as a terminology authority.

---

# 6. Arabic Writing Quality

Arabic content must be authored as natural educational Arabic, not word-for-word translated English.

- Use clear Modern Standard Arabic suitable for professional learners.
- Avoid unnecessarily academic, archaic, or machine-translated phrasing.
- English technical terms should appear where they help the learner recognize terminology encountered in professional tools such as Meta Ads, Google Ads, GA4, analytics platforms, documentation, and job materials.
- Examples may be localized when that improves comprehension, but must not change the learning objective or factual meaning.

---

# 7. Review

Arabic instructional content remains subject to the applicable content review gates: all four quality gates in OA-GOV-001 §6 plus the Arabic linguistic check in OA-GOV-001 §9. No gate may be skipped or combined.

The review must additionally verify:

- fidelity to the canonical English source;
- Arabic clarity and naturalness;
- terminology consistency with the Canonical Terminology Glossary;
- preservation of learning objectives, scope, and sequence;
- preservation of assignment and rubric meaning;
- no unsupported new claims introduced during adaptation.

This standard does not weaken any existing review gate.

---

# 8. Arabic UI Copy

Arabic learner-facing UI copy is NOT instructional lesson content and does not require the full lesson-content review lifecycle.

It is governed by the Arabic UI Copy Review gate defined in OA-GOV-001 §9: a lightweight Product Owner review for:

- Arabic correctness and clarity;
- terminology consistency with the Canonical Terminology Glossary;
- contextual/action correctness;
- no unintended English learner-facing copy;
- no change to API or business semantics.

English may appear learner-facing only where intentionally retained as canonical technical terminology (ADR-0008).

---

# 9. Out of Scope

This standard does NOT authorize:

- i18n architecture
- a language switcher
- a `preferred_language` database field
- multilingual database storage
- runtime locale selection
- a translation library
- an English/Arabic learner toggle

OA-BL-027 remains deferred to v0.2.

This standard also does not, by itself, authorize the production of any specific Arabic adaptation. Adaptation production work is tasked separately.

---

# 10. Compliance Checklist

Before an Arabic Learning Adaptation is submitted for review, verify:

- [ ] Anchored to the approved English lesson content itself, not only to its brief.
- [ ] Learning objectives, scope, sequence, and required concepts preserved.
- [ ] Assignment intent, required deliverable, and rubric meaning preserved.
- [ ] Terminology follows the Canonical Terminology Glossary; the first-use pattern is applied.
- [ ] Natural Modern Standard Arabic; no machine-translation phrasing.
- [ ] Mandatory source metadata block present per ADR-0004 (Source, Source Version, Translation Version, Sync Status, Last Synced).
- [ ] No unsupported new claims introduced during adaptation.
