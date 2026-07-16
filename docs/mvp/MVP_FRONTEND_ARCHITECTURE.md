# MVP Frontend Architecture

**Document ID:** OA-MVP-009
**Version:** 1.0.0
**Status:** Approved
**Date:** 2026-07-16
**Reference:** OA-MVP-001 MVP Product Brief, OA-MVP-002 MVP Information Architecture, OA-MVP-003 MVP User Flows, OA-MVP-004 MVP Wireframes, OA-MVP-007 MVP API Contracts, OA-MVP-008 MVP Backend Architecture

---

## Purpose

This document defines the frontend architecture for Version 0.1 of Oravil Academy. It describes the layered structure, navigation model, state management strategy, component organization, and the constraints that govern how the frontend is built. It does not specify technology, framework, styling, or implementation.

---

## Architectural Style

Version 0.1 uses a feature-oriented component architecture with a clear separation between presentation, state, and API communication. Each screen defined in OA-MVP-004 is the primary organizational unit, and the components within it are organized around the feature they serve rather than their technical type.

This approach is appropriate for Version 0.1 because the product has a small, fixed set of screens and a linear user journey. The architecture needs to be readable by any developer from day one, minimize the risk of state inconsistency in a sequentially gated flow, and keep the server as the authoritative source of all learning progress. Optimizing for clarity and correctness is more valuable than optimizing for scale at this stage.

---

## Design Principles

1. Server as source of truth — learner progress, lesson content, and completion state are always sourced from the API; the frontend never derives or assumes these values independently.
2. Feature-oriented organization — code is grouped by the screen or feature it serves, not by technical role (components, hooks, utilities scattered across the codebase).
3. Explicit navigation — route transitions are always the result of a deliberate user action or a confirmed server response; the frontend never navigates speculatively.
4. Predictable state — application state is updated in response to explicit events; no implicit state mutation occurs as a side effect of rendering.
5. Thin presentation — components describe what the user sees and what actions they can take; they do not contain data-fetching logic, API calls, or business decisions.
6. Single responsibility — each component has one purpose; a component that renders a lesson list does not also manage submission state.
7. Accessible by default — interactive elements are keyboard-navigable and screen-reader-compatible by construction, not by retrofit.

---

## High-Level Layers

### Presentation Layer

Contains all components responsible for rendering UI. Presentation components receive data and callbacks as inputs and emit user interaction events as outputs. They have no knowledge of where data comes from or how it is stored. They do not call APIs. They do not read from or write to application state directly.

### Application Layer

Contains the logic that connects user interactions to state changes and API calls. The Application layer translates user events from the Presentation layer into API requests or state mutations, handles responses, and passes results back to the Presentation layer. This is where navigation decisions are made — for example, after a successful assignment submission, the Application layer decides whether to navigate to the next lesson or to the Module Complete screen based on the server response.

### State Layer

Contains the structured representation of the data the frontend holds at any given moment. State is organized by feature domain — module state, lesson state, progress state, submission state, survey state. The State layer does not derive business logic; it stores and exposes what the server has confirmed. Components read from state; they do not write to it directly.

### API Layer

Contains the functions responsible for communicating with the backend as defined in OA-MVP-007. Each function corresponds to one endpoint. The API layer handles request construction, response parsing, and error normalization. It does not contain business logic. The Application layer calls the API layer; the Presentation layer does not.

---

## Navigation Model

Navigation follows the sequential flow defined in OA-MVP-003 exactly. The frontend enforces the same progression rules at the navigation level that the backend enforces at the API level:

- The learner cannot navigate to a locked lesson. Any attempt to access a locked lesson route redirects to the Module Overview.
- The Module Complete screen is only reachable after the server confirms that all four assignments have been submitted.
- The Post-Module Survey screen is only reachable from the Module Complete screen.
- Back navigation returns the learner to the Module Overview, never to a previous lesson in progress.
- After survey submission, no forward navigation is available.

Route resolution always begins with a progress check against the server. The frontend does not cache or assume the learner's current position between sessions; it fetches fresh progress state on each entry to the application.

---

## State Management

State is divided into three categories based on its origin, scope, and lifetime.

### Local UI State

Owned by a single component and not shared. Examples: whether a text input has been touched, whether a loading spinner is visible, whether a validation message is currently shown. Local UI state is discarded when the component is unmounted. It never leaves the Presentation layer.

### Application State

Shared across multiple components within a feature or screen. Examples: the current lesson's content after it has been fetched, the list of lessons and their statuses on the Module Overview, the current submission form content. Application state is managed by the Application layer and passed down to Presentation components. It is reset when the learner navigates away from its owning feature.

### Server State

The canonical state of the learner's progress, submissions, and survey responses as confirmed by the backend. Server state is fetched from the API on demand — on initial load, after a successful submission, and on return from a previous session. The frontend never modifies server state directly; it submits an action to the API and then re-fetches to reflect the confirmed result. Server state supersedes any locally cached application state.

---

## Component Organization

The frontend is organized by feature domain, mirroring the screen inventory defined in OA-MVP-004.

**Module Overview** — components responsible for displaying the module title, purpose, deliverable description, lesson list with status indicators, and the primary action. Reads module and progress state.

**Lesson View** — components responsible for rendering the full lesson content, including the workshop section delimiter and the assignment CTA at the end of the lesson. Reads lesson state.

**Assignment Submission** — components responsible for displaying the assignment prompt, capturing the learner's written response, showing validation feedback, and submitting. Manages local form state and delegates submission to the Application layer.

**Module Complete** — components responsible for displaying the completion confirmation, the completed lesson list, the deliverable reminder, and the survey CTA. Reads module and progress state.

**Post-Module Survey** — components responsible for rendering survey questions by type (rating scale, open text), capturing answers, showing validation feedback, and submitting. Manages local form state and delegates submission to the Application layer.

**Shared** — components used across two or more feature domains: the global header, the module progress indicator, loading state indicators, and the standard error message display.

---

## Cross-Cutting Concerns

### Authentication

On application load, the frontend verifies that a valid token exists. If no token is present or the token is rejected by the API with a 401 response, the learner is redirected to the access entry point. The token is attached to every API request by the API layer. No component or Application layer function is responsible for token management.

### Loading States

Every API call is accompanied by a loading state managed at the Application layer. Presentation components receive a loading flag and render an appropriate indicator while the request is in flight. No component renders with empty or stale data; it renders in a loading state until data is confirmed.

### Error States

API errors are caught by the API layer and normalized into a consistent error shape before being passed to the Application layer. The Application layer maps error types to the appropriate UI response: 401 triggers re-authentication, 403 triggers a redirect to the Module Overview, 404 triggers a not-found message, 422 surfaces field-level validation messages. Unhandled errors display a generic error message. No raw error detail is shown to the learner.

### Validation Feedback

Client-side validation mirrors the rules defined in OA-MVP-007. Validation messages are shown only after the learner has attempted to submit, never before. Field-level messages returned in a 422 response from the API are displayed adjacent to the relevant input. The submit action is re-enabled after the learner corrects the input.

### Accessibility

All interactive elements — buttons, text inputs, rating controls — are operable by keyboard. Focus is managed explicitly on route transitions and modal state changes. All form inputs have associated labels. All status indicators (locked, available, complete) carry text descriptions, not only visual indicators. The reading progress indicator on the Lesson View is purely informational and does not require interaction.

---

## Architectural Constraints

1. Presentation components must not call the API layer directly.
2. Presentation components must not read from or write to application state directly.
3. Presentation components must not contain navigation logic.
4. The Application layer must not render UI elements.
5. The API layer must not contain business logic or state management.
6. Navigation must always be the result of a user action or a confirmed server response, never a rendering side effect.
7. Server state must always be re-fetched after a mutating API call; the frontend must not optimistically update server state.
8. No component may derive learner progress from local state; progress must always be sourced from the server.
9. Local UI state must not escape its owning component.
10. The frontend must enforce the same navigation gating rules defined in OA-MVP-003 regardless of direct URL access.
11. Validation feedback must only appear after a submission attempt; pre-emptive validation is not permitted.
12. All API errors must be caught and normalized at the API layer boundary; raw error objects must not reach the Presentation layer.
13. Authentication token management must be handled exclusively by the API layer.
14. Shared components must have no dependency on any single feature domain's state.
15. Every route transition must begin with a server state fetch; the frontend must not resume from a locally cached position.

---

## Future Expansion

The architecture is organized around the screen and feature domain as the primary units, not around a specific set of content. Adding a second module requires adding new instances of the existing five feature domains — Module Overview, Lesson View, Assignment Submission, Module Complete, Post-Module Survey — populated with the new module's data. No structural change to the architecture is required. Adding a new learning path is identical in cost. If a new screen type is introduced in a future version — for example, a learning path overview or a learner profile screen — it is added as a new feature domain alongside the existing ones, following the same layered structure and constraints. The Shared component domain grows to accommodate new cross-cutting UI needs without modifying any existing feature domain.
