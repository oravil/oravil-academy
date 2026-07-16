# MVP Wireframes

**Document ID:** OA-MVP-004
**Version:** 1.0.0
**Status:** Approved
**Date:** 2026-07-16
**Reference:** OA-MVP-001 MVP Product Brief, OA-MVP-002 MVP Information Architecture, OA-MVP-003 MVP User Flows

---

## Purpose

This document defines the structural composition of every screen in Version 0.1 of Oravil Academy. It specifies what appears on each screen, in what order, and what action it supports. It does not define visual design, styling, color, typography, or implementation.

---

## Design Principles

1. One primary action per screen — every screen has exactly one forward action; secondary actions are limited to reading previously completed content.
2. Content before navigation — lesson content is presented first; structural chrome is minimal.
3. Progressive disclosure — learners see only the content and actions relevant to their current state.
4. Consistent layout — all screens share the same structural shell so the learner builds orientation from the first screen.
5. Completion-oriented — every screen ends with a clear next step; no screen leaves the learner without direction.

---

## Screen 1 — Module Overview

**Purpose:** Orient the learner to Module 1 before they begin, and show current progress on return visits.

**Layout Structure (top to bottom):**

1. Global header
2. Module title and phase context
3. Module purpose statement
4. Module deliverable description
5. Lesson list with completion status
6. Primary action

**Visible Components:**

- Module title: Phase 0 — Module 1: The Digital Marketing Landscape
- One-sentence module purpose
- Module deliverable label and brief description
- Lesson list — four items, each showing: lesson number, lesson title, and status (locked / in progress / complete)
- Primary action button

**Primary Action:**

- If no lessons are complete: Begin Lesson 1
- If one or more lessons are complete but the module is not finished: Continue to [current lesson]
- If all lessons are complete: Proceed to Module Complete

**Empty States:**

- First visit: all lessons show as locked except Lesson 1, which shows as available.

---

## Screen 2 — Lesson View

**Purpose:** Deliver the full lesson content to the learner in a single, scrollable reading experience.

**Layout Structure (top to bottom):**

1. Global header
2. Lesson title and lesson number
3. Lesson metadata (estimated reading time, module context)
4. Lesson content body — full sequential lesson as authored
5. Workshop section — embedded within the content body, clearly delimited
6. Assignment instructions — final section of the content body
7. Primary action

**Visible Components:**

- Lesson title
- Lesson number and module reference
- Estimated reading time
- Full lesson content in authored order: Lesson Information, Learning Objectives, Business Context, Core Concepts, Professional Thinking, Inside Oravil, Practical Example, Hands-on Workshop, Assignment, Deliverable, Common Mistakes, Troubleshooting, Knowledge Check, Lesson Summary, Additional Resources, Next Lesson
- A visual delimiter marking the Workshop section within the lesson
- Assignment instructions and deliverable specification at the end of the lesson
- Primary action to proceed to assignment submission

**Reading Progress Indicator:**

A simple indicator showing the learner's scroll position within the lesson. This is informational only and does not gate the primary action.

**Workshop Placement:**

The Workshop is rendered inline within the lesson content body, immediately following the Practical Example section, consistent with the lesson structure defined in OA-STD-001.

**Assignment CTA:**

The assignment instructions are the final substantive section of the lesson. Immediately following the lesson content, a clearly delimited block presents the assignment deliverable specification and the primary action to submit.

**Primary Action:**

Proceed to Assignment Submission for this lesson.

---

## Screen 3 — Assignment Submission

**Purpose:** Allow the learner to write and submit the assignment deliverable for the current lesson.

**Layout Structure (top to bottom):**

1. Global header
2. Assignment context — lesson number, lesson title, deliverable name
3. Assignment prompt — the exact deliverable specification from the lesson
4. Text input area
5. Validation message area
6. Primary action

**Visible Components:**

- Lesson number and title
- Deliverable name
- Full assignment prompt as written in the lesson
- Text input area for the learner's submission
- Character or word count indicator (informational only)
- Validation message area — visible only when validation is triggered
- Primary action button

**Validation Messages:**

- Empty submission: "Your assignment is empty. Please write your response before submitting."
- Submission below minimum length (if a minimum is defined for the lesson): "Your response is shorter than the minimum required. Please complete your answer."
- No validation message is shown before the learner attempts to submit.

**Primary Action:**

Submit assignment.

---

## Screen 4 — Module Complete

**Purpose:** Confirm to the learner that all four lessons and all four assignments have been completed, and direct them to the post-module survey.

**Layout Structure (top to bottom):**

1. Global header
2. Completion confirmation heading
3. Module summary — title, four lessons listed with completed status
4. Module deliverable reminder — brief statement noting that the learner has built the Digital Marketing Landscape Map across the four lessons
5. Next step prompt
6. Primary action

**Visible Components:**

- Completion confirmation heading
- Module title: Phase 0 — Module 1: The Digital Marketing Landscape
- Four lessons listed, each marked complete
- One-sentence module deliverable reminder
- Prompt directing the learner to complete the post-module survey
- Primary action button

**Primary Action:**

Proceed to post-module survey.

---

## Screen 5 — Post-Module Survey

**Purpose:** Collect a satisfaction rating and qualitative written feedback from the learner following module completion.

**Layout Structure (top to bottom):**

1. Global header
2. Survey introduction — one sentence explaining the purpose
3. Question 1 — satisfaction rating
4. Question 2 — qualitative feedback
5. Question 3 — optional open question
6. Primary action

**Visible Components:**

- Survey introduction
- Question 1: "How would you rate your learning experience in Module 1?" — 1 to 5 scale with labels at each end (1: Poor, 5: Excellent)
- Question 2: "What would you change or improve about this module?" — open text input
- Question 3: "Is there anything else you want to share?" — open text input, optional
- Primary action button

**Primary Action:**

Submit survey.

After submission, the learner sees a single confirmation message stating that Module 1 is complete and their feedback has been received. No further action is available in Version 0.1.

---

## Global Elements

The following elements appear on every screen:

**Header:**
- Product name: Oravil Academy
- Current context label: Phase 0 — Module 1: The Digital Marketing Landscape
- No additional navigation items in Version 0.1

**Module Progress Indicator:**
- A minimal indicator showing how many of the four lessons have been completed (for example: Lesson 2 of 4)
- Shown on all screens except the Post-Module Survey and the final confirmation state

**No breadcrumb is used in Version 0.1.** The current context label in the header provides sufficient orientation within the constrained scope of a single module.

---

## Navigation Behavior

- From Module Overview, the learner moves forward to the current lesson's Lesson View.
- From Lesson View, the learner moves forward to the Assignment Submission screen for that lesson.
- From Assignment Submission, on successful submission, the learner moves forward to the Module Overview (if lessons remain) or to the Module Complete screen (if Lesson 4 was just submitted).
- From Module Complete, the learner moves forward to the Post-Module Survey.
- From the Post-Module Survey, on submission, the learner reaches the final confirmation state. No further forward navigation is available.
- The learner may navigate back to the Module Overview at any time.
- The learner may open any previously completed lesson to re-read its content.
- The learner cannot navigate forward past their current progress state.

---

## Future Expansion

All five screens in this document are defined around the module as the primary structural unit. When additional modules are added, Screens 1, 2, 3, and 4 require only a content update — the module title, lesson list, and deliverable description change, but the layout structure and component composition remain identical. Screen 5 is module-agnostic and reusable without modification. A learning path overview screen may be introduced in a later version to sit above the Module Overview, but this does not require changes to any screen defined here.
