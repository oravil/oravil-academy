# MVP Domain Model

**Document ID:** OA-MVP-005
**Version:** 1.0.0
**Status:** Approved
**Date:** 2026-07-16
**Reference:** OA-MVP-001 MVP Product Brief, OA-MVP-002 MVP Information Architecture, OA-MVP-003 MVP User Flows, OA-MVP-004 MVP Wireframes

---

## Purpose

This document defines the core business entities of Version 0.1 of Oravil Academy, their responsibilities, and the relationships between them. It establishes the language of the domain and the rules that govern it. It is technology-independent and does not describe persistence, APIs, or implementation details.

---

## Design Principles

1. Business concepts first — entities represent real things in the learning domain, not technical constructs.
2. Technology independent — no entity implies a database table, API resource, or class hierarchy.
3. Single responsibility — each entity owns one clearly bounded concern.
4. Stable relationships — relationships between entities reflect the structure of the learning methodology and do not change with content.
5. Minimal domain — only entities required to validate the methodology in Version 0.1 are included.

---

## Core Entities

### Learner

**Purpose:** Represents a person enrolled in the MVP who progresses through Module 1.

**Owns:**
- A set of Assignment Submissions
- A Survey Response
- A Progress record

**Belongs To:**
- Nothing — the Learner is a root entity in this domain.

---

### Learning Path

**Purpose:** Represents a named, structured sequence of educational content organised around a professional discipline. In Version 0.1, the only Learning Path is Digital Marketing.

**Owns:**
- One or more Phases

**Belongs To:**
- Nothing — the Learning Path is a root entity in this domain.

---

### Phase

**Purpose:** Represents a major stage of progression within a Learning Path, grouping related Modules around a shared skill level or theme. In Version 0.1, the only Phase is Phase 0 — Foundations.

**Owns:**
- One or more Modules

**Belongs To:**
- One Learning Path

---

### Module

**Purpose:** Represents a self-contained unit of instruction that produces a defined deliverable. A Module is the primary completion unit in Version 0.1. The only Module in scope is Module 1 — The Digital Marketing Landscape.

**Owns:**
- An ordered sequence of Lessons
- A Module Deliverable definition

**Belongs To:**
- One Phase

---

### Lesson

**Purpose:** Represents a single, structured piece of instructional content that answers one learning question and produces one assignment deliverable. Lessons are consumed in a fixed sequence.

**Owns:**
- One Assignment

**Belongs To:**
- One Module

---

### Assignment

**Purpose:** Represents the written deliverable that a Learner must produce and submit to demonstrate understanding of the Lesson. Each Assignment has a defined prompt and a minimum response requirement.

**Owns:**
- A deliverable prompt
- A minimum response specification

**Belongs To:**
- One Lesson

---

### Assignment Submission

**Purpose:** Represents a Learner's response to a specific Assignment at a point in its lifecycle. A submission moves through defined states; the domain model does not assume a single permanent state.

**Owns:**
- Submission Content — the written response provided by the Learner
- Submission Status — the current lifecycle state of the submission

**Belongs To:**
- One Learner
- One Assignment

---

### Progress

**Purpose:** A derived view representing the Learner's progression through a Module. Progress reflects confirmed learning events — specifically, Assignment Submissions in a Submitted status and a completed Survey Response — and does not independently store learning state beyond what those events establish.

**Owns:**
- A record of completed Lessons
- A current Lesson reference
- A Module completion state
- A survey completion state

**Belongs To:**
- One Learner

---

### Survey

**Purpose:** Represents the post-module feedback instrument presented to the Learner after Module 1 is complete. In Version 0.1, there is one Survey associated with Module 1.

**Owns:**
- A set of Questions

**Belongs To:**
- One Module

---

### Survey Response

**Purpose:** Represents a Learner's submitted answers to the post-module Survey. A Survey Response is final once submitted.

**Owns:**
- A satisfaction rating
- One or more open-text answers

**Belongs To:**
- One Learner
- One Survey

---

## Entity Relationships

One Learning Path contains one or more Phases.

One Phase contains one or more Modules.

One Module contains one or more Lessons in a fixed sequence.

One Lesson owns exactly one Assignment.

One Module owns exactly one Survey.

One Learner has exactly one Progress record per Module.

One Learner has one Assignment Submission per Assignment, created when the Learner submits their response.

One Learner has one Survey Response per Survey, created when the Learner submits the post-module survey.

One Assignment Submission belongs to exactly one Learner and exactly one Assignment.

One Survey Response belongs to exactly one Learner and exactly one Survey.

One Progress record reflects the state of exactly one Learner within exactly one Module.

---

## Domain Rules

1. A Lesson belongs to exactly one Module and cannot exist independently.
2. An Assignment belongs to exactly one Lesson and cannot exist independently.
3. Lessons within a Module are ordered and must be completed in sequence.
4. A Lesson is considered complete when an Assignment Submission exists for that Lesson's Assignment.
5. A Module is complete only when Assignment Submissions exist for all Lessons in that Module.
6. The Survey for a Module becomes available only when the Module is complete.
7. The valid submission lifecycle for Version 0.1 consists of a single Submitted status. The domain model does not preclude additional lifecycle states in future versions, but no other status is defined or active in Version 0.1.
8. A Survey Response is final. Once submitted, it cannot be modified or deleted.
9. A Learner has at most one Assignment Submission per Assignment.
10. A Learner has at most one Survey Response per Survey.
11. A Learner's Progress reflects only confirmed, submitted states. Partial reading state is not recorded in Version 0.1.

---

## Aggregate Boundaries

An aggregate is a cluster of entities treated as a single unit for the purposes of consistency.

**Learner Aggregate (root: Learner)**
The Learner is the root of an aggregate that includes Progress, all Assignment Submissions belonging to that Learner, and all Survey Responses belonging to that Learner. Consistency rules governing what a Learner can submit and what state changes are valid are enforced at this boundary.

**Module Aggregate (root: Module)**
The Module is the root of an aggregate that includes its ordered Lessons, each Lesson's Assignment, and the Module's Survey. The Module is the authority on its own structure, sequence, and completion definition. No external entity may alter the Module's lesson sequence or assignment definitions.

**Learning Path Aggregate (root: Learning Path)**
The Learning Path is the root of an aggregate that includes its Phases and, by extension, their Modules. The Learning Path defines the valid sequence of Phases and the overall structure of the educational programme.

---

## Future Expansion

The domain model is intentionally defined around the Learning Path, Phase, and Module as generic structural containers. Adding new learning paths requires only the creation of new Learning Path, Phase, and Module entities — no changes to the Learner, Progress, Assignment Submission, Survey, or Survey Response entities are required. The relationship rules remain unchanged. A Learner's Progress record is scoped to a Module, so supporting multiple modules or multiple learning paths requires only that additional Progress records be created for each Module a Learner enrols in. The aggregate boundaries are stable across any number of learning paths and modules.
