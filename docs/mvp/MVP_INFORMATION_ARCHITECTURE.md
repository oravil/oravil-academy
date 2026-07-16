# MVP Information Architecture

**Document ID:** OA-MVP-002
**Version:** 1.0.0
**Status:** Approved
**Date:** 2026-07-16
**Reference:** OA-MVP-001 MVP Product Brief

---

## Purpose

This document defines how information is structured and navigated in Version 0.1 of Oravil Academy. It establishes the content hierarchy, screen inventory, and navigation rules that govern the learner experience. It does not define implementation, technology, or visual design.

---

## Design Principles

1. Content first — the lesson is the product; everything else serves it.
2. Sequential progression — learners move through content in a defined order without exception.
3. Completion as the primary action — every screen has one clear next step.
4. No unnecessary navigation — learners cannot reach content they have not yet unlocked.
5. Minimal cognitive load — no extraneous options, menus, or decisions at any point in the journey.

---

## Information Hierarchy

```
Platform
    ↓
Learning Path
    ↓
Phase
    ↓
Module
    ↓
Lesson
    ↓
Workshop
    ↓
Assignment
```

Each level contains only what is needed to support the level below it. The MVP exposes one path through this hierarchy: Digital Marketing → Phase 0 → Module 1 → Lessons 1–4.

---

## Primary Navigation

The following navigation items are visible to the learner during the MVP:

- Module Overview
- Current Lesson
- Assignment Submission
- Module Complete
- Post-Module Survey

No global navigation, no course catalogue, and no account settings are exposed in Version 0.1.

---

## Screen Inventory

### Module Overview

**Purpose:** Orient the learner to Module 1 before beginning.
**Primary User Action:** Begin Lesson 1.

---

### Lesson View

**Purpose:** Deliver the full lesson content to the learner, including Business Context, Core Concepts, Workshop, and Assignment instructions.
**Primary User Action:** Mark lesson as read and proceed to assignment submission.

---

### Assignment Submission

**Purpose:** Allow the learner to submit the written deliverable for the current lesson.
**Primary User Action:** Submit assignment.

---

### Module Complete

**Purpose:** Confirm that all four lessons and all four assignments have been submitted.
**Primary User Action:** Proceed to post-module survey.

---

### Post-Module Survey

**Purpose:** Collect satisfaction rating and qualitative feedback on the module experience.
**Primary User Action:** Submit survey.

---

## Content Relationships

The Digital Marketing learning path contains Phase 0. Phase 0 contains Module 1. Module 1 contains four lessons in a fixed sequence. Each lesson contains a Workshop embedded within the lesson content and an Assignment that the learner submits separately. The Module Completion state is reached only after all four assignments have been submitted. The Post-Module Survey is the final action and closes the learner's engagement with Version 0.1.

Each object in the hierarchy has a single parent and does not exist independently:

- A lesson belongs to exactly one module.
- An assignment belongs to exactly one lesson.
- A module is complete only when all of its lessons are complete.
- A learning path phase is complete only when all of its modules are complete.

---

## Navigation Rules

1. Lessons unlock sequentially. Lesson 2 is not accessible until Lesson 1 is marked complete.
2. Lesson 3 is not accessible until the Lesson 2 assignment is submitted.
3. Lesson 4 is not accessible until the Lesson 3 assignment is submitted.
4. The Module Complete screen is not accessible until all four assignments are submitted.
5. The Post-Module Survey is not accessible until the Module Complete screen has been reached.
6. Learners may revisit any completed lesson.

Assignments remain editable until the testing window closes.

Only the latest submission is evaluated.
7. Learners can return to any previously completed lesson to re-read the content at any time.

---

## Future Expansion

The hierarchy defined in this document — Platform → Learning Path → Phase → Module → Lesson → Workshop → Assignment — is intentionally generic. Adding a second learning path in Version 0.2 or later requires only the addition of a new Learning Path node at the top of the hierarchy. No structural change is needed to the Phase, Module, or Lesson levels. Navigation rules apply uniformly to any learning path using the same content structure, so new paths inherit the same sequential progression model without modification. The Post-Module Survey and Module Completion screens are reusable across all modules by design.
