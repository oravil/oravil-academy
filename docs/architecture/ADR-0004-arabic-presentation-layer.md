# ADR-0004 — Arabic Presentation Layer

> Document ID: OA-ADR-0004
> Status: Approved
> Date: 2026-07-16
> Supersedes: Operational language decisions in ADR-0003 (section: MVP Pilot Language and First Localization Target only)

---

## Scope of Supersession

This ADR supersedes only the operational language decisions recorded in ADR-0003:

- MVP Pilot Language
- First Localization Target

It does NOT supersede, modify, or replace:

- Canonical authoring language (English)
- Repository language (English)
- Documentation language (English)

Those decisions remain governed by ADR-0003 and are not revisited here.

---

## Context

ADR-0003 established English as the canonical authoring language and recorded Arabic as the first localization target after MVP validation.

The MVP pilot is delivered to a professional Arabic-speaking audience. Requiring learners to engage in English during a methodology validation pilot would introduce unnecessary friction and produce unreliable signal.

This ADR defines the operational model for Arabic content production: how Arabic content is derived from English sources, how it is structured in the repository, how version alignment is maintained, and what lifecycle stages govern its production.

---

## Decision

### Arabic as a Derived Presentation Layer

Arabic content is a derived presentation layer, not an independent source.

Every Arabic lesson is produced from an approved English source. Arabic content is never authored independently. The English source remains the canonical record for all content decisions.

If a conflict exists between an Arabic lesson and its English source, the English source is authoritative.

### File Structure

Lessons that have Arabic renderings use a language-directory structure:

```
module-directory/
    lesson-01/
        en.md
        ar.md
    lesson-02/
        en.md
        ar.md
```

Each language variant is stored as a separate file inside a named lesson directory.

This structure is adopted as a scalability decision. As additional languages are introduced, each is added as a new file inside the existing lesson directory without renaming or reorganizing existing files. The lesson directory name is the stable identifier. The language code is the variant selector.

### Source Metadata

Every Arabic lesson file must begin with the following metadata block:

```
Source: [relative path to the English source file]
Source Version: [version of the English source at time of rendering]
Translation Version: [version of this Arabic file]
Sync Status: [Synced | Pending Update]
Last Synced: [date]
```

These fields are mandatory. No Arabic lesson is considered valid without them.

The Sync Status field is authoritative for determining whether an Arabic lesson reflects the latest approved English source. A status of Pending Update means the English source has been revised and the Arabic rendering has not yet been updated. A status of Pending Update blocks Arabic publication of the affected lesson until the rendering is updated and reviewed.

### Publication Lifecycle

Arabic content follows the lifecycle below:

```
Approved (EN)
↓
Arabic Rendering
↓
Arabic Review
↓
Pilot Publication (AR)
```

The English source is an internal canonical artifact. English lessons are not published to learners during the MVP pilot. The pilot is delivered in Arabic.

### Derivation Rule

Arabic files are never edited independently.

Any modification to Arabic content must originate from an approved change in the English source.

The workflow is:

1. An approved change is made to the English source.
2. The Arabic rendering is updated to reflect the change.
3. The Arabic review stage is completed.
4. The Sync Status metadata is updated to Synced.
5. The Arabic lesson is re-published.

No change to an Arabic file is valid unless a corresponding change exists in the approved English source.

### Terminology Glossary

The canonical terminology glossary for all supported languages is maintained at:

```
docs/references/GLOSSARY.md
```

This document is the single terminology authority for every language variant produced within this repository. When a term is translated into Arabic, the approved translation is recorded in this glossary. No other document serves as a terminology authority.

---

## Rationale

**Derivation prevents divergence.**

Treating Arabic as a derived layer, rather than a parallel source, enforces a single point of truth. Content cannot silently diverge across languages when one language is always derived from the other.

**Language-directory structure scales without disruption.**

A file naming convention such as `lesson.ar.md` cannot accommodate a third language without renaming files or breaking existing references. A directory structure with language-coded files supports any number of languages by adding a new file. Existing files, paths, and references remain stable.

**Mandatory source metadata enables systematic maintenance.**

As the content library grows, manual tracking of which Arabic lessons are current becomes unreliable. Explicit metadata in every file makes the sync state machine-readable and auditable without external tooling.

**The pilot is Arabic, not English.**

ADR-0003 stated the MVP pilot would run in English only. This decision is superseded. Requiring the Arabic-speaking target audience to engage in English during a methodology validation pilot would invalidate the methodology signal. The pilot must be delivered in the language of its intended audience.

**Production effort is proportional, not doubled.**

Production effort increases because every approved English lesson requires an additional rendering and review stage before Arabic publication. This is a known and accepted cost of serving the target audience from the first release.

---

## Consequences

- The lesson directory structure must be applied to all lessons that require an Arabic rendering.
- English-only lessons (produced before Arabic rendering begins) use a flat file structure and are migrated to the directory structure when Arabic rendering is introduced.
- Every Arabic lesson file must carry the mandatory source metadata fields defined in this ADR.
- The Content Lifecycle (OA-GOV-001) must be updated to include the Arabic rendering and Arabic review stages.
- `docs/references/GLOSSARY.md` must be established and maintained as the single terminology authority.
- The Product Backlog item for Arabic localization capability is considered active under this ADR.

---

## Status

Approved.
