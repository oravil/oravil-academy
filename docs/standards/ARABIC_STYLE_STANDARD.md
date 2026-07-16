# ARABIC STYLE STANDARD

> Document ID: OA-STD-010
> Status: Approved
> Version: 1.0.0
> Date: 2026-07-16
> Owner: Oravil Academy

---

# 1. Purpose

This standard defines the voice, register, sentence style, example localisation rules, and terminology policy for all Arabic content produced within Oravil Academy.

It is the authoritative review standard for the Arabic Review stage of the Content Lifecycle (OA-GOV-001).

Every Arabic rendering must comply with this standard before it may be approved for pilot publication.

---

# 2. Register

Arabic lessons use **simplified professional Arabic** (Modern Standard Arabic simplified for professional readability).

The standard rejects both extremes:

- **No classical (fusha) register.** Classical Arabic creates unnecessary distance between the learner and practical content.
- **No full colloquial register.** Colloquial Arabic is regional, reduces credibility, and cannot scale across Arabic-speaking markets.

The target register reads as a knowledgeable professional speaking clearly to a capable peer — not as a textbook, and not as informal conversation.

---

# 3. Sentence Style

- Sentences are short. One idea per sentence.
- Active voice is preferred over passive voice.
- Avoid nominal chains (a sequence of nouns where a verb phrase would be clearer).
- Paragraphs are short. Three to four sentences is the maximum for instructional prose.
- Bullet lists are used for enumerations of three or more items.
- Headers and section titles follow the same hierarchy as the English source.

---

# 4. Example Localisation

All examples in Arabic renderings must use **Egyptian-market examples** and references unless the English source specifically references a global standard (such as Google Ads, Meta Ads Manager, or an internationally recognised brand) that requires no localisation.

Localisation rules:

- Brand names in examples are replaced with well-known Egyptian or regional equivalents where relevant.
- Currency references use Egyptian Pound (EGP) unless the context requires a different currency.
- Market size, consumer behaviour, and industry examples reflect the Egyptian or broader Arab market.
- Business scenarios reference locally recognisable industry sectors: e-commerce, food and beverage, real estate, retail.

A lesson that uses an example referencing a US-only business context or a US-centric statistic must have that example replaced or adapted for the Egyptian market, not merely translated.

---

# 5. Industry Term Retention Policy

Industry terms that are universally used in English within the Arabic-speaking marketing profession are **retained in English** inside Arabic text.

Examples: SEO, CPA, CPC, CPM, PPC, UTM, KPI, ROI, SEM.

Rules for retained terms:

1. The term appears in English on every use.
2. On the **first occurrence** in a lesson, the term is followed immediately by its Arabic explanation in parentheses.
   - Example: SEO (تحسين محركات البحث)
3. Subsequent occurrences use the English term only.
4. The approved Arabic explanation for each retained term is the translation recorded in `docs/references/GLOSSARY.md`.
5. A reviewer may not introduce a new Arabic translation for a retained term without first updating the glossary.

---

# 6. Terminology Consistency

All translations of non-retained terms must match the approved translations in `docs/references/GLOSSARY.md`.

A reviewer encountering an unapproved translation must:

1. Flag the inconsistency during the Arabic Review stage.
2. Either apply the glossary translation or propose a glossary update through the standard review process.

A lesson may not be published with terminology that contradicts the glossary.

---

# 7. Meaning Parity

The Arabic rendering must convey the same instructional meaning as the approved English source.

Permitted during rendering:

- Adapting examples for the Egyptian market (per Section 4).
- Restructuring sentences for Arabic grammar.
- Inserting first-occurrence term explanations (per Section 5).

Not permitted during rendering:

- Adding content not present in the English source.
- Removing content present in the English source.
- Changing learning objectives, assignment prompts, or deliverable definitions.
- Interpreting or editorialising the source.

Any meaning discrepancy identified during Arabic Review is a blocking issue. The English source is authoritative in all cases of conflict.

---

# 8. Acceptance Criteria for Arabic Review

An Arabic rendering passes review when all of the following are confirmed:

- Register complies with Section 2 (simplified professional Arabic; no classical; no colloquial).
- Sentence style complies with Section 3.
- All examples comply with Section 4 (Egyptian-market localisation applied).
- Industry terms comply with Section 5 (retained in English with first-occurrence explanation).
- All translated terms match the approved entries in `docs/references/GLOSSARY.md`.
- Meaning parity with the English source is confirmed (Section 7).
- No content has been added or removed.
- The source metadata block at the top of the Arabic file is complete and accurate.

---

# 9. Scope

This standard applies to:

- All Arabic lesson files (`ar.md`) produced under the language-directory structure defined in ADR-0004.
- Arabic assignment prompts seeded into the database.
- Any other learner-facing Arabic text produced by or for Oravil Academy.

This standard does not apply to:

- Repository governance documents (always English per ADR-0003).
- Internal production artifacts: lesson briefs, author notes, ADRs, standards.
