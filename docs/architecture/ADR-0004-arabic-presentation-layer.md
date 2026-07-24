# ADR-0004 — Arabic Presentation Layer

> Document ID: OA-ADR-0004
> Status: Approved
> Date: 2026-07-16
> Supersedes: Operational language decisions in ADR-0003 (section: MVP Pilot Language and First Localization Target only)
> Amended: 2026-07-24 — derived presentation layer broadened to the Arabic Learning Adaptation model; see ADR-0008
> Amended: 2026-07-24 — file-structure pattern extended to module presentation, survey, and (source-driven) assignment derivatives; Product Owner decision

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

The derived presentation layer produces an **Arabic Learning Adaptation** of each approved English lesson. The Arabic artifact is authored against the approved English lesson — it is neither a literal translation nor a free re-authoring. Rewording and localized examples are permitted. The learning objectives, scope, sequence, required deliverable, and rubric of the English source must not change.

The derived presentation layer covers the full learner-facing experience, not lesson content alone. ADR-0008 (Arabic Learner Presentation) defines the Version 0.1 presentation scope: an Arabic-only learner surface, with English retained as the canonical language for all authoring, specifications, code, APIs, and documentation.

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

### Non-Lesson Learner-Facing Derivatives

The derived presentation layer is not limited to lesson bodies. Other learner-facing content requires the same derivation model — a canonical English source, and a separate Arabic derivative file, never authored independently. Product Owner decision (2026-07-24) extends the file-structure pattern above, minimally, to three additional artifact types:

**Module presentation.** The Module Brief (`MODULE_BRIEF.md`) remains the canonical English curriculum/specification artifact and is not translated. Where a module has learner-facing presentation fields (title, purpose, deliverable description — the fields a learner sees on the Module Overview and Module Complete screens), the Arabic derivative is a sibling file at the module directory root:

```
module-directory/
    MODULE_BRIEF.md
    ar.md
    lesson-01/
        en.md
        ar.md
```

`module-directory/ar.md` contains only the learner-facing presentation fields derived from `MODULE_BRIEF.md` — never the Module Brief's internal planning sections (scope, exit criteria, dependencies, etc.), which the learner never sees.

**Survey.** The governing English survey definition (currently `docs/mvp/MVP_WIREFRAMES.md`, an MVP specification document) remains canonical and is not translated in place — ADR-0003's Repository Language decision keeps specification documents English. The Arabic derivative lives in a dedicated `survey/` directory under the module:

```
module-directory/
    survey/
        ar.md
```

`survey/ar.md` contains only the persisted, learner-facing seeded survey content — the survey title and question text. It does not contain question types, required/optional flags, ordering keys, or any other machine-readable value; those remain defined solely in the canonical English source and the platform schema.

**Assignments.** No blanket per-lesson assignment derivative is introduced. The rule is source-driven, not lesson-number-driven: where a lesson's persisted assignment content (prompt and deliverable name) is deterministically derivable from that lesson's own `## Deliverable` section and Hands-on Workshop step title, the existing `lesson-0X/ar.md` already serves as the Arabic derivative — no separate file is created. Where the persisted assignment content instead originates from a source other than the lesson body (for example, the Lesson Brief's `## Deliverable` field, when the lesson body has no `## Deliverable` section of its own), a dedicated derivative is required:

```
module-directory/
    lesson-01/
        en.md
        ar.md
        assignment.ar.md
```

`lesson-0X/assignment.ar.md` is created only for lessons whose audited seeded-assignment source is not the lesson body itself. Its presence or absence per lesson is a consequence of where that lesson's persisted assignment content actually originates, not a fixed rule that every lesson gets one.

### Source Metadata

Every Arabic file governed by this ADR — lesson (`ar.md`), module presentation (`ar.md`), survey (`survey/ar.md`), and assignment derivatives (`assignment.ar.md`) alike — must begin with the following metadata block:

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
- Product Owner decision (2026-07-24): the derivative model extends, minimally, to module presentation (`module-directory/ar.md`), survey content (`module-directory/survey/ar.md`), and — where the source audit requires it — assignment content (`lesson-0X/assignment.ar.md`). This is a targeted extension of the existing lesson-level pattern to the v0.1 content already identified as learner-facing (ADR-0008), not a general localization architecture. It does not introduce locale tables, translation tables, `preferred_language`, a language switcher, an i18n library, or multilingual API contracts. OA-BL-027 remains deferred to v0.2.

---

## Status

Approved.
