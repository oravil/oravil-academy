# PROJECT EXECUTION GUIDE

**Document ID:** OA-GUIDE-001
**Version:** 1.0.0
**Status:** Master Document
**Date:** 2026-07-16
**Owner:** Oravil Academy Product Owner

---
How to Continue This Project



1. Read PROJECT_EXECUTION_GUIDE.md completely.
2. Read the relevant ADRs.
3. Read the target artifact specification.
4. Do not modify approved documents directly.
5. Record improvements in PRODUCT_BACKLOG.md.
6. Follow the Implementation Plan.
7. Preserve traceability.
8. Keep the MVP scope intact.
9. Ask before changing architecture.
10. Documentation is the source of truth.


# 1. Executive Summary

Oravil Academy is a professional learning platform designed to teach real-world technical and business disciplines through structured, production-grade educational content.

Unlike traditional online courses that focus primarily on information delivery, Oravil Academy is built around measurable competency. Every lesson is designed to move the learner from understanding to application through business context, structured reasoning, guided practice, and practical deliverables.

The platform itself is not the product.

The methodology is the product.

Version 0.1 exists solely to validate that methodology before investing in platform scale, automation, AI features, or commercial expansion.

Every architectural, educational, and product decision made within this repository supports that single objective.

---

# 2. Project Vision

The long-term vision of Oravil Academy is to become a professional education platform where learners acquire practical, industry-ready skills through structured learning paths built to the same standards used in software engineering and product development.

The platform is intended to produce practitioners rather than certificate holders.

Success is measured by demonstrated competency, not by lesson completion.

Over time the Academy will expand into multiple professional disciplines while maintaining one consistent methodology for curriculum design, assessment, and learner progression.

---

# 3. Project Mission

The mission of Version 0.1 is intentionally narrow.

Validate that the Oravil educational methodology enables beginner learners to understand concepts, apply them to realistic business situations, and produce meaningful deliverables.

Nothing else is considered success.

If the methodology is validated, the platform expands.

If the methodology is not validated, the methodology changes before the platform grows.

---

# 4. Project Scope

Version 0.1 includes only one learning path:

Digital Marketing

Within that learning path only:

Phase 0 — Foundations

Within that phase only:

Module 1 — The Digital Marketing Landscape

The MVP intentionally excludes:

- certificates
- gamification
- AI tutoring
- dashboards
- multiple learning paths
- community features
- payments
- subscriptions
- mobile applications

Every excluded feature is deferred until methodology validation has been completed.

---

# 5. Success Definition

Version 0.1 is successful only if:

- learners complete the module,
- learners submit meaningful assignments,
- assignments demonstrate understanding,
- learners report a positive educational experience,
- collected feedback identifies clear opportunities for improvement.

Feature completeness alone is not considered success.

The educational methodology is the primary hypothesis under validation.

---

# 6. Core Philosophy

The following principles govern every decision made within the project.

## Product Before Technology

Technology exists to implement the product.

The product never changes to accommodate technology.

---

## Methodology Before Scale

A scalable system teaching the wrong methodology is still a failure.

Educational quality always precedes technical scale.

---

## Learning Before Features

Every new feature must improve learning.

If it does not improve learning, it belongs in the backlog until justified.

---

## Validate Before Expand

Expansion occurs only after evidence.

Ideas do not justify implementation.

Validated outcomes justify implementation.

---

## Simplicity Before Complexity

Every solution begins with the smallest architecture capable of validating the hypothesis.

Complexity must be earned.

---

## Documentation Before Code

Documentation defines intent.

Code implements documentation.

Whenever implementation and documentation disagree, documentation is considered authoritative until formally updated.

---

## Business Rules Before Frameworks

Business rules belong to the product.

Frameworks exist only to execute those rules.

Changing frameworks must never change business behaviour.
# 7. Educational Philosophy

Oravil Academy does not teach topics.

It develops professional competency.

Every lesson is designed around the belief that understanding is demonstrated through application rather than recall.

For this reason, every educational artifact exists for a specific instructional purpose and follows a defined lifecycle.

Learning progresses from understanding to execution through structured stages rather than isolated content.

The educational philosophy of the Academy is built on the following principles.

---

## Business Context First

Every lesson begins with a realistic business situation.

The learner should understand why the topic matters before learning how it works.

Concepts are introduced because they solve real problems, not because they exist academically.

---

## Concept Before Tool

Tools change.

Concepts remain.

The Academy teaches durable mental models before introducing platforms, software, or implementation details.

A learner who understands the concept can adapt to any tool.

A learner who memorizes a tool cannot necessarily solve a business problem.

---

## Progressive Learning

Every lesson assumes only the knowledge acquired in previous lessons.

No lesson introduces concepts that have not yet been prepared.

Learning always moves forward through controlled progression.

---

## Active Learning

Reading alone is insufficient.

Every lesson requires the learner to:

- think,
- classify,
- compare,
- analyze,
- produce,
- reflect.

Passive consumption is intentionally avoided.

---

## Practical Application

Every lesson produces a tangible outcome.

Examples include:

- comparison tables,
- business analyses,
- definitions,
- planning documents,
- strategy maps,
- recommendations.

Knowledge becomes valuable only when transformed into work.

---

## Reflection Before Assessment

Learners are encouraged to reason before being evaluated.

Workshops exist to strengthen thinking.

Assignments exist to demonstrate competency.

Assessment never introduces new concepts.

It validates concepts already learned.

---

## Professional Thinking

The Academy trains learners to think like practitioners rather than students.

Lessons emphasize:

- decision making,
- trade-offs,
- business reasoning,
- communication,
- structured thinking.

Memorization is never the educational objective.

---

## Incremental Mastery

Each lesson contributes to a larger deliverable.

Lessons are not isolated.

By completing all lessons in a module, learners gradually build a professional artifact that demonstrates integrated understanding.

---

# 8. Content Production Philosophy

Educational content is treated as a product rather than documentation.

Every lesson follows a controlled production lifecycle.

No lesson is written directly.

Content always progresses through the following stages:

Backlog

↓

Module Brief

↓

Lesson Brief

↓

Research

↓

Lesson Production

↓

Technical Review

↓

Educational Review

↓

Approval

↓

Publication

Each stage exists to improve quality while reducing inconsistency across the curriculum.

No stage may be skipped.

---

## Research Before Writing

Every lesson begins with research.

Research identifies:

- authoritative definitions,
- accepted terminology,
- scope boundaries,
- misconceptions,
- business context,
- professional references.

Lesson production begins only after research has been reviewed.

---

## Brief Before Production

A Lesson Brief defines:

- purpose,
- objectives,
- deliverables,
- dependencies,
- assessment,
- scope.

The lesson is an implementation of the brief.

The brief is never reverse-engineered from the lesson.

---

## One Source of Truth

Every educational decision exists in exactly one location.

Duplicated specifications are avoided.

Lesson Briefs define lessons.

Module Briefs define modules.

Standards define formatting.

The Lifecycle defines workflow.

The Constitution defines governance.

---

## Continuous Improvement

Educational improvements are expected.

However, approved lessons are never modified arbitrarily.

Improvement proposals are recorded in the Product Backlog.

Changes are reviewed before becoming part of future versions.

The curriculum evolves intentionally rather than continuously.

# 9. Product Philosophy

Oravil Academy is built as a product, not as a collection of educational documents.

Every decision begins with the learner and the educational objective rather than the technology used to deliver it.

The platform exists to validate and deliver the methodology—not to showcase engineering complexity.

The following principles govern every product decision.

---

## Content Is the Product

The primary value delivered by Oravil Academy is its educational content.

The application exists to present, organize, and support that content.

If every feature disappeared except the lessons, the core product would still exist.

If the lessons disappeared, the platform would have no value.

Therefore, content receives priority over interface, infrastructure, and automation.

---

## The Methodology Is the Competitive Advantage

Many platforms can host courses.

The differentiator of Oravil Academy is not content hosting.

It is the methodology used to transform information into professional competency.

Every lesson follows the same instructional structure.

Every module follows the same educational architecture.

Every learning path follows the same progression model.

Consistency is intentional.

---

## Educational Quality Over Feature Quantity

New features are never added because they are common.

Every feature must answer one question:

"Does this improve learning?"

If the answer is unclear, the feature belongs in the backlog until evidence exists.

Version 0.1 intentionally excludes many common LMS features because they do not contribute to validating the methodology.

---

## Sequential Learning

Learning is intentionally linear.

A learner progresses by demonstrating understanding rather than simply opening content.

Each completed lesson unlocks the next.

The learner always knows:

- where they are,
- what has been completed,
- what comes next.

The platform minimizes unnecessary decisions during learning.

---

## Minimal Cognitive Load

Every screen has one primary purpose.

Navigation is intentionally limited.

Visual complexity is avoided.

The learner should spend cognitive effort understanding the lesson—not understanding the interface.

---

## Deliverables Over Completion

Reading is not completion.

Watching is not completion.

Completion occurs only when the learner produces work.

Assignments are therefore central to the learning experience rather than optional additions.

---

## Evidence-Based Evolution

Every version of the Academy is treated as a product experiment.

Feedback is collected.

Assignments are reviewed.

Completion rates are measured.

Only then are improvements prioritized.

The roadmap follows evidence rather than opinion.

---

# 10. Product Lifecycle

Every feature introduced into Oravil Academy follows a controlled lifecycle.

Ideas do not move directly into development.

Instead they progress through structured stages that reduce uncertainty and maintain consistency.

The lifecycle is:

Idea

↓

Product Backlog

↓

Research

↓

Product Definition

↓

Architecture

↓

Implementation Planning

↓

Development

↓

Testing

↓

Pilot Release

↓

Feedback Collection

↓

Evaluation

↓

Next Version

No stage may be skipped without explicit approval.

---

## MVP First

Every new initiative begins with the smallest version capable of validating the intended hypothesis.

The MVP must answer one clear question.

If the MVP cannot answer that question, it is too large.

---

## Pilot Before Scale

No feature is considered complete until real learners have used it.

Internal testing verifies correctness.

Pilot testing verifies usefulness.

Both are required.

---

## Feedback Before Expansion

Expansion is based on observed behaviour.

Suggestions alone are insufficient.

Every major roadmap decision should reference:

- learner completion data,
- assignment quality,
- satisfaction scores,
- qualitative feedback.

---

## Versioning Philosophy

Versions represent validated milestones rather than arbitrary schedules.

Version numbers advance only when meaningful objectives have been achieved.

A version is complete when:

- its objectives are met,
- documentation is updated,
- implementation is stable,
- lessons learned have been incorporated into future planning.

The project evolves through validated iterations rather than continuous uncontrolled change.

# 11. Curriculum Architecture

The curriculum is organized as a hierarchical learning system.

Each level has a single responsibility and exists only to support the level beneath it.

The hierarchy is intentionally stable so that new learning paths can be added without changing the educational model.

The curriculum hierarchy is:

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

Each level is described below.

---

## Platform

The Platform is the highest-level container.

Its responsibility is to provide a consistent learning experience across all educational disciplines offered by Oravil Academy.

The platform itself contains no educational content.

It provides the environment in which learning paths are delivered.

---

## Learning Path

A Learning Path represents a complete professional discipline.

Examples include:

- Digital Marketing
- Software Engineering
- UI/UX Design
- Cybersecurity

Each Learning Path is independent.

A learner may complete multiple learning paths without affecting progress in another.

Learning Paths are the highest educational unit visible to learners.

---

## Phase

A Phase groups related skills into a logical stage of professional development.

Phases establish progression.

Each phase prepares the learner for the next.

For example:

Digital Marketing

↓

Phase 0 — Foundations

↓

Phase 1 — Content and SEO

↓

Phase 2 — Paid Media

↓

...

A Phase is complete only after all of its Modules have been completed.

---

## Module

A Module is the primary instructional unit.

Each Module focuses on one coherent competency.

A Module contains:

- a defined purpose,
- measurable learning outcomes,
- one or more lessons,
- one integrated deliverable,
- clear exit criteria.

Modules are intentionally self-contained.

Completing a Module should result in a tangible professional capability.

---

## Lesson

A Lesson answers one educational question.

Every Lesson has one clearly defined instructional objective.

Lessons follow a standardized structure defined by the Academy Standards.

Each Lesson contributes directly to the Module Deliverable.

Lessons never duplicate one another.

---

## Workshop

The Workshop is embedded within the Lesson.

Its purpose is guided practice.

Workshops prepare learners for the Assignment by encouraging reasoning, classification, comparison, or analysis before formal assessment.

Workshop completion is part of learning.

Assignment completion is part of validation.

---

## Assignment

Every Lesson ends with one Assignment.

Assignments require learners to produce original work.

Assignments demonstrate application rather than recall.

Examples include:

- written definitions,
- comparison tables,
- recommendations,
- planning documents,
- analysis reports,
- strategy maps.

Assignments are cumulative.

Together they construct the Module Deliverable.

---

# 12. Curriculum Design Rules

Every curriculum produced by Oravil Academy follows these rules.

---

## One Competency Per Module

A Module teaches one professional competency.

Large topics are divided into multiple Modules.

Modules should never become miniature courses.

---

## One Learning Question Per Lesson

Every Lesson answers one primary question.

If a lesson attempts to answer multiple unrelated questions, it should be divided.

---

## Progressive Dependencies

Lessons may depend only on earlier lessons.

Future knowledge must never be assumed.

Every dependency is explicit.

---

## Clear Scope Boundaries

Every Module Brief and Lesson Brief defines:

- Included
- Not Included

This prevents scope creep and duplication.

---

## Deliverable-Driven Learning

Every Module produces exactly one integrated deliverable.

Every Lesson contributes a portion of that deliverable.

Learning accumulates progressively.

---

## Assessment Aligns With Objectives

Assignments evaluate only the stated learning objectives.

No assignment introduces new concepts.

Assessment validates learning.

It does not create learning.

---

## Professional Context Is Mandatory

Every Lesson begins with a realistic business context.

Learners understand why the topic matters before studying how it works.

Professional reasoning is developed alongside technical knowledge.

---

## Standards Before Creativity

Authors have freedom within constraints.

Every lesson may differ in examples and explanations.

However, structure, terminology, workflow, and educational quality remain consistent across the Academy.

Consistency creates trust.

Variation creates engagement.

Both are required.

# 13. Product Architecture

The product architecture defines how the learner experiences Oravil Academy from first access to module completion.

It is intentionally simple.

The learner should never need to understand the platform in order to use it.

Instead, every screen should answer one question:

> "What is the next thing the learner should do?"

This philosophy governs every product decision.

---

## Information Hierarchy

The learner moves through a fixed educational hierarchy.

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

Each level exists solely to organize the level beneath it.

The hierarchy is stable across every learning path.

---

## Primary User Journey

The learner journey is intentionally linear.

```
Module Overview

↓

Lesson

↓

Workshop

↓

Assignment Submission

↓

Next Lesson

↓

Module Completion

↓

Post-Module Survey
```

Every transition has one clear forward action.

No unnecessary navigation exists.

---

## Product Screens

Version 0.1 contains only five functional screens.

### Module Overview

Purpose:

Orient the learner before beginning the module.

Responsibilities:

- Present module purpose.
- Show lesson progression.
- Display current learner status.
- Provide one primary action.

---

### Lesson View

Purpose:

Deliver the educational content.

Responsibilities:

- Present lesson content.
- Present workshop.
- Present assignment.
- Guide the learner toward submission.

---

### Assignment Submission

Purpose:

Capture the learner's work.

Responsibilities:

- Display assignment prompt.
- Validate submission.
- Record learner response.

---

### Module Complete

Purpose:

Confirm successful completion.

Responsibilities:

- Summarize completed lessons.
- Reinforce module deliverable.
- Direct learner to survey.

---

### Post-Module Survey

Purpose:

Collect methodology validation data.

Responsibilities:

- Capture learner satisfaction.
- Capture qualitative feedback.
- Close learner journey.

---

# 14. Software Architecture

The software architecture exists to support the educational methodology.

Technology is an implementation detail.

Architecture is permanent.

Frameworks may change.

Business rules should not.

The architecture follows a layered model.

```
Presentation

↓

Application

↓

Domain

↓

Infrastructure

↓

Persistence
```

Each layer owns a single responsibility.

Dependencies always point inward.

---

## Domain First

The Domain Model is the heart of the system.

Everything else exists to support it.

Business rules belong here.

Examples include:

- lesson progression
- assignment completion
- module completion
- survey eligibility

No other layer may redefine these rules.

---

## API First

Frontend and backend communicate exclusively through approved API contracts.

No component may depend on undocumented behavior.

API contracts are written before implementation.

Implementation follows contracts.

---

## Server as Source of Truth

The backend owns:

- learner progress
- lesson availability
- assignment state
- survey state

The frontend displays server state.

It never invents state.

---

## Feature-Oriented Design

The system is organized around business capabilities rather than technical layers.

Examples include:

- Learning Path
- Module
- Lesson
- Assignment
- Progress
- Survey
- Learner

Each feature owns its own business behavior.

---

## Vertical Slice Development

Implementation progresses feature by feature.

Each slice includes:

Database

↓

Domain

↓

Application

↓

API

↓

Frontend

↓

Testing

↓

Review

Every completed slice produces working software.

---

# 15. Repository Organization

The repository is organized around documentation first and implementation second.

Every directory has a single responsibility.

This repository (`oravil-academy`) is a **documentation and curriculum repository**.

Application source code — backend, frontend, database migrations, and infrastructure — lives in the separate `oravil-academy-platform` repository. See ADR-0002 for the rationale behind this decision.

Typical structure of this repository:

```
docs/
    constitution/
    standards/
    curriculum/
    mvp/
    architecture/
    backlog/

scripts/
```

The documentation hierarchy must remain stable.

---

## Documentation Before Implementation

Every significant implementation artifact must be traceable back to documentation.

Examples:

Feature

↓

Product Brief

↓

Architecture

↓

Implementation

↓

Code

↓

Tests

If traceability is lost, architecture has drifted.

---

## Separation of Concerns

Documentation is not mixed with implementation.

Curriculum is not mixed with software architecture.

Architecture is not mixed with infrastructure.

Each concern has one canonical location.

---

# 16. Document Hierarchy

Documents have authority levels.

When two documents appear to conflict, the document higher in the hierarchy is considered authoritative.

```
Academy Constitution

↓

Lifecycle

↓

Standards

↓

Project Execution Guide

↓

ADR

↓

Product Brief

↓

Architecture Documents

↓

Implementation Plan

↓

Code

↓

Tests
```

Code never overrides documentation.

Documentation must be updated before implementation changes are accepted.

---

## Single Source of Truth

Every decision exists exactly once.

Examples:

Curriculum structure

→ Module Brief

Lesson objectives

→ Lesson Brief

Architecture

→ Architecture Documents

API behavior

→ API Contracts

Database rules

→ Database Schema

Implementation order

→ Implementation Plan

No duplicate specifications should exist across multiple documents.

---

## Stable Documents

Approved documents are considered stable.

Corrections may be made.

Structural changes require review.

New ideas belong in the Product Backlog until approved.

This keeps the repository predictable and prevents silent architectural drift.

# 17. Development Workflow

Every implementation activity within Oravil Academy follows a defined workflow.

The workflow exists to ensure consistency, maintain architectural integrity, and prevent undocumented implementation decisions.

No contributor—human or AI—may bypass these stages.

The workflow is:

```
Idea

↓

Product Backlog

↓

Research

↓

Specification

↓

Architecture

↓

Implementation Plan

↓

Development

↓

Testing

↓

Review

↓

Approval

↓

Release
```

Each stage has a single responsibility.

---

## Stage 1 — Idea

Every improvement begins as an idea.

Ideas are intentionally lightweight.

No implementation discussion occurs at this stage.

An idea answers only one question:

"What problem are we trying to solve?"

---

## Stage 2 — Product Backlog

Approved ideas enter the Product Backlog.

The backlog is the only location where future work is prioritized.

Nothing is developed directly from conversation or memory.

Every future enhancement must first exist in the backlog.

---

## Stage 3 — Research

Research validates assumptions before design begins.

Research may include:

- business analysis,
- educational research,
- user feedback,
- technical feasibility,
- competitor analysis.

Research reduces uncertainty.

It does not make implementation decisions.

---

## Stage 4 — Specification

The specification defines exactly what will be built.

Depending on the artifact, this may include:

- Product Brief
- Module Brief
- Lesson Brief
- MVP Document
- ADR

Development never begins without an approved specification.

---

## Stage 5 — Architecture

Architecture determines how the specification will be implemented.

Architecture defines:

- domain boundaries,
- data ownership,
- API contracts,
- responsibilities,
- dependencies.

Architecture intentionally avoids implementation details.

---

## Stage 6 — Implementation Planning

Implementation planning converts architecture into executable work.

It determines:

- development order,
- testing strategy,
- definition of done,
- release criteria.

Planning minimizes implementation risk.

---

## Stage 7 — Development

Development implements the approved plan.

Developers do not redesign the product during implementation.

Unexpected discoveries result in documentation updates rather than undocumented code changes.

---

## Stage 8 — Testing

Testing verifies that implementation satisfies the specification.

Testing occurs at multiple levels:

- unit,
- integration,
- end-to-end,
- manual acceptance.

Testing validates behavior.

It does not define behavior.

---

## Stage 9 — Review

Every completed artifact is reviewed.

Review confirms:

- correctness,
- consistency,
- architectural compliance,
- educational quality,
- documentation accuracy.

Review is mandatory.

---

## Stage 10 — Approval

Only approved work becomes part of the project baseline.

Approval indicates that the artifact satisfies all applicable standards.

---

## Stage 11 — Release

Release makes the approved work available to its intended audience.

For educational content:

Publication.

For software:

Deployment.

For documentation:

Repository integration.

Release is the end of one lifecycle and the beginning of the next feedback cycle.

---

# 18. Git Workflow

Git history is considered part of the project documentation.

Every commit should explain *why* the repository changed rather than simply *what* changed.

---

## Commit Philosophy

Commits should be:

- atomic,
- reviewable,
- traceable,
- reversible.

Each commit should represent one logical unit of work.

Large mixed-purpose commits are discouraged.

---

## Commit Format

The project follows Conventional Commits.

Examples:

```
docs(curriculum): define Module 1 lesson briefs

docs(mvp): define API contracts

docs(architecture): define backend architecture

feat(progress): implement learner progression

fix(api): validate assignment submissions

refactor(domain): simplify progress calculation
```

---

## Branch Strategy

Version 0.1 may be developed directly on the primary branch while the project remains in early development.

As the project grows, feature branches become the preferred workflow.

Recommended naming:

```
feature/module-overview

feature/lesson-view

feature/assignment-submission

feature/progress

feature/survey
```

---

## Pull Request Philosophy

Every Pull Request should answer four questions:

1. What problem is solved?
2. Which approved document does this implement?
3. How was it tested?
4. Does it introduce any architectural change?

If architecture changes, the corresponding documentation must be updated before approval.

---

## Traceability

Every implementation should be traceable back to documentation.

Example:

```
Feature

↓

API Contract

↓

Backend Architecture

↓

Frontend Architecture

↓

Implementation

↓

Tests
```

If traceability cannot be established, implementation should be reviewed before merging.

---

# 19. Coding Philosophy

Although frameworks and languages may change, the following principles remain constant.

---

## Business Rules Live in the Domain

Business behavior belongs exclusively to the Domain layer.

Controllers, UI components, repositories, and infrastructure must never redefine business rules.

---

## Thin Interfaces

Controllers and UI components coordinate work.

They do not contain business decisions.

Their responsibility is orchestration.

---

## Explicit Validation

Validation should occur as early as possible.

Invalid input should never reach business logic.

Validation rules should be centralized and consistent.

---

## Predictable Code

Code should prioritize readability over cleverness.

Future maintainers should understand intent without reverse engineering.

---

## Single Responsibility

Every component should have one reason to change.

Responsibilities should remain clearly separated.

---

## Composition Over Duplication

Shared behavior should be composed rather than copied.

Duplication increases maintenance cost and inconsistency.

---

## Documentation and Code Stay Together

Whenever implementation changes expected behavior, the corresponding documentation must be reviewed.

Code and documentation should evolve together rather than independently.

# 20. AI Agent Operating Manual

This section defines the mandatory operating rules for any AI agent contributing to the Oravil Academy repository.

The objective is continuity.

A new AI agent should be able to continue the project without changing its philosophy, architecture, documentation standards, or implementation approach.

The AI agent is expected to behave as a disciplined engineering contributor rather than an autonomous decision-maker.

---

## Primary Responsibility

The AI agent exists to assist the Product Owner in executing the approved project.

Its responsibility is to:

- implement approved work,
- improve quality,
- maintain consistency,
- identify risks,
- suggest improvements.

The AI agent is **not** responsible for redefining product strategy.

---

## Source of Truth

The AI agent must always work from approved documentation.

Priority order (highest authority first):

1. Academy Constitution
2. Content Lifecycle
3. Standards
4. Project Execution Guide
5. Architecture Decision Records (ADR)
6. Approved Product Documents
7. Architecture Documents
8. Implementation Plan
9. Product Backlog

This order matches the Document Hierarchy defined in Section 16.

Conversation history is never considered authoritative.

Repository documentation is.

---

## Before Starting Any Task

Before producing output the AI agent must determine:

- What artifact is being modified?
- Is the artifact already approved?
- Which document governs this artifact?
- Does the requested work already exist in the backlog?
- Will the requested work change architecture?

If any answer is unclear, clarification should occur before implementation.

---

## Approved Documents

Approved documents are considered frozen.

The AI agent may:

- correct mistakes,
- improve wording,
- clarify ambiguity.

The AI agent must not:

- redesign,
- restructure,
- expand scope,
- silently change behavior.

Structural changes require an approved ADR or Product Backlog item.

---

## Product Backlog Policy

Whenever the AI agent identifies:

- an improvement,
- a future feature,
- a refactoring opportunity,
- a better UX,
- a scalability enhancement,

the proposal should be recorded as a backlog candidate rather than implemented immediately.

Ideas are collected before they are executed.

---

## MVP Discipline

Version 0.1 exists for validation.

The AI agent must actively protect the MVP from unnecessary expansion.

The following are rejected unless explicitly approved:

- gamification,
- AI tutoring,
- achievements,
- dashboards,
- certificates,
- social features,
- advanced analytics,
- monetization,
- notifications beyond MVP scope.

Every proposed feature must answer:

> Does this improve methodology validation?

If not, it belongs in the backlog.

---

## Documentation First

Whenever implementation changes expected behavior:

documentation changes first.

Implementation follows.

Documentation is never generated from code.

Code is generated from documentation.

---

## Architectural Integrity

The AI agent must preserve the approved architecture.

Examples:

- Business rules remain in the Domain.
- Controllers remain thin.
- API contracts remain authoritative.
- Frontend never becomes the source of truth.
- Features remain vertically integrated.

No implementation convenience justifies architectural drift.

---

## Communication Style

The AI agent should communicate like a senior engineering partner.

Responses should be:

- direct,
- technically accurate,
- concise,
- traceable,
- evidence-based.

Recommendations should distinguish between:

- required changes,
- optional improvements,
- future backlog items.

Opinion should never be presented as fact.

---

## Decision Escalation

The AI agent may independently decide:

- formatting improvements,
- wording improvements,
- implementation details already covered by documentation,
- non-breaking refactoring suggestions.

The AI agent must request Product Owner approval before:

- changing architecture,
- expanding scope,
- changing business rules,
- changing educational methodology,
- introducing new technologies,
- changing repository organization,
- modifying approved workflows.

---

## Completion Checklist

Before considering any task complete, the AI agent should verify:

- Documentation remains consistent.
- No approved rule has been violated.
- Scope has not expanded unintentionally.
- The implementation matches the specification.
- Naming remains consistent.
- Terminology follows project standards.
- Traceability has been preserved.

Only then should the task be considered complete.

---

# 21. Review and Approval Process

Every artifact produced within Oravil Academy follows the same review model regardless of whether it is educational content, documentation, architecture, or implementation.

Review protects consistency.

Approval establishes the new project baseline.

---

## Review Objectives

Review verifies:

- correctness,
- completeness,
- consistency,
- clarity,
- architectural compliance,
- educational compliance,
- maintainability.

Review is intended to improve quality rather than assign blame.

---

## Review Levels

### Level 1 — Author Review

The creator confirms:

- requirements are satisfied,
- formatting is correct,
- references are valid,
- obvious mistakes are removed.

---

### Level 2 — Technical Review

The reviewer confirms:

- technical correctness,
- architectural consistency,
- compliance with standards,
- terminology consistency.

---

### Level 3 — Product Review

The Product Owner confirms:

- alignment with product goals,
- scope compliance,
- business correctness,
- roadmap consistency.

Only after Product Review is the artifact considered approved.

---

## Definition of Approved

An artifact is approved only when:

- review comments are resolved,
- no blocking issues remain,
- documentation is internally consistent,
- affected documents have been updated,
- the Product Owner accepts the result.

Approval establishes the new baseline.

Future work begins from that baseline.

---

## Continuous Improvement

Approval does not imply perfection.

Future improvements are expected.

However, improvements follow the same lifecycle:

Idea

↓

Backlog

↓

Review

↓

Approval

↓

Implementation

The repository evolves through controlled improvement rather than continuous uncontrolled modification.

# 22. Release Management

Releases within Oravil Academy represent validated milestones rather than arbitrary delivery dates.

A release is approved only when it satisfies its documented objectives and has passed all required review stages.

The release process exists to preserve product quality, educational consistency, and architectural integrity.

---

## Release Philosophy

A release is considered successful only when:

- the intended educational objective has been achieved,
- implementation matches the approved specification,
- documentation reflects the implemented system,
- quality gates have been satisfied,
- the Product Owner authorizes publication.

Feature count is not a release metric.

Educational effectiveness is.

---

## Release Lifecycle

Every release follows the same lifecycle.

```
Planning

↓

Specification

↓

Architecture

↓

Implementation

↓

Testing

↓

Review

↓

Pilot

↓

Feedback

↓

Evaluation

↓

Release
```

Each stage produces evidence that the next stage may begin.

No stage should be skipped.

---

## Pilot Release

Version 0.1 is intentionally released to a limited audience.

The purpose of the pilot is validation rather than growth.

The pilot collects evidence regarding:

- learner completion,
- assignment quality,
- educational clarity,
- usability,
- learner satisfaction,
- implementation stability.

Pilot data drives future planning.

---

## Feedback Collection

Feedback is collected from multiple sources.

Examples include:

- learner surveys,
- assignment reviews,
- completion statistics,
- observed usability issues,
- reviewer observations.

Feedback should be classified as:

- Bug
- Improvement
- Enhancement
- New Feature
- Documentation Issue

Only validated feedback influences future releases.

---

## Version Progression

Version numbers represent meaningful project milestones.

A version advances only when:

- objectives are achieved,
- quality gates are passed,
- documentation is current,
- implementation is stable,
- review is complete.

Versions are not tied to dates.

They are tied to validated outcomes.

---

## Release Documentation

Every release should include:

- release objectives,
- completed backlog items,
- known limitations,
- architectural decisions,
- outstanding backlog items,
- lessons learned.

Release documentation becomes part of the permanent project history.

---

# 23. Versioning Strategy

Version numbers communicate project maturity.

The following progression model is adopted.

---

## Version 0.x

Purpose:

Methodology validation.

Characteristics:

- limited scope,
- controlled audience,
- rapid learning,
- architecture stabilization.

Version 0.x prioritizes evidence over expansion.

---

## Version 1.x

Purpose:

Production readiness.

Characteristics:

- complete learner experience,
- multiple modules,
- operational stability,
- scalable architecture.

Version 1.x focuses on delivering a complete educational product.

---

## Version 2.x

Purpose:

Platform expansion.

Characteristics:

- additional learning paths,
- richer learner experience,
- advanced analytics,
- operational optimization.

Expansion occurs only after Version 1 objectives have been achieved.

---

## Breaking Changes

Breaking changes require:

- updated documentation,
- updated ADR when applicable,
- version increment,
- migration strategy,
- Product Owner approval.

Backward compatibility should be preserved whenever practical.

---

# 24. Current Project Status

This section provides the current baseline for the repository.

It should be updated only when a major project milestone is completed.

---

## Repository Status

### Governance

✅ Complete

### Educational Standards

⚠️ Partial

OA-STD-001 (Lesson Standard) published.

Deferred: Content, Quiz, Lab, Case Study, SOP, Markdown, Naming, and Design System standards. See PRODUCT_BACKLOG.md.

### Curriculum Design

✅ Complete for:

Digital Marketing

↓

Phase 0

↓

Module 1

↓

Lessons 1–4

---

### Product Definition

✅ Complete

---

### Software Architecture

✅ Complete

---

### Implementation Planning

✅ Complete

---

### Development

⏳ Not Started

---

### Testing

⏳ Not Started

---

### Pilot Release

⏳ Not Started

---

## Current MVP Scope

Learning Path

- Digital Marketing

Phase

- Phase 0 — Foundations

Module

- Module 1 — The Digital Marketing Landscape

Lessons

- Lesson 1 — What Is Digital Marketing
- Lesson 2 — Digital vs. Traditional Marketing
- Lesson 3 — Core Digital Marketing Channels
- Lesson 4 — Digital Marketing Careers

---

## Current Repository Baseline

The following documents define the current approved baseline.

### Governance

- Constitution
- Standards
- Lifecycle
- Product Backlog

### Curriculum

- Learning Path
- Phase Definitions
- Module Briefs
- Lesson Briefs
- Lesson Research
- Lesson Production

### Product

- MVP Product Brief
- Information Architecture
- User Flows
- Wireframes

### Architecture

- Domain Model
- Database Schema
- API Contracts
- Backend Architecture
- Frontend Architecture
- Implementation Plan
- ADR-0001
- ADR-0002
- ADR-0003

---

## Current Development Phase

Planning

✅ Complete

Architecture

✅ Complete

Implementation

⏳ Ready to Begin

---

## Immediate Next Objective

Begin Sprint 1 implementation using the approved Vertical Slice strategy defined in the Implementation Plan.

No additional product or architectural documentation is required before implementation unless a new architectural decision becomes necessary.

---

# 25. Closing Statement

This document is the operational handbook for Oravil Academy.

Its purpose is not merely to describe the project, but to preserve the intent, philosophy, standards, and decision-making process behind it.

A contributor who understands this guide should be able to continue the project without access to prior conversations, personal context, or undocumented assumptions.

Every future decision should align with the principles established here.

When uncertainty arises, contributors should favor:

- educational quality over feature quantity,
- clarity over cleverness,
- documentation over assumption,
- validation over speculation,
- consistency over convenience.

Oravil Academy is built to evolve deliberately.

The methodology comes first.

The product exists to deliver the methodology.

The technology exists to serve the product.

This order should never change.

---

**End of Document**
