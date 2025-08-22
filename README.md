Project Summary (DriverPass)

Client: DriverPass, a small driving-instruction business (represented by the owner/operations lead).
Goal: Design an information system to manage end-to-end driver training—students purchasing packages, scheduling lessons with instructors, taking practice tests, and admins overseeing operations.

Scope & Core Features

Student portal: account creation, purchase training packages, schedule/cancel lessons, view progress, take DMV-style practice tests, receive reminders.

Instructor portal: manage availability, accept/decline lessons, record lesson notes, see route/vehicle info.

Admin portal: manage users, vehicles, packages/pricing, calendars, test banks; run reports (utilization, pass rates, cancellations).

System services: role-based access control, notifications (email/SMS), payment logging, audit trails, analytics dashboard.

Non-functional priorities: security (HTTPS, salted password hashes, least-privilege RBAC), reliability (99.5% target uptime), performance (p95 < 500ms for common flows), privacy (minimal PII, retention policy), and maintainability (modular services, API docs).

What I Did Particularly Well

Requirements → Design Traceability: I captured stakeholder goals as user stories with acceptance criteria, then mapped each to use cases, domain objects, and UI flows. The traceability matrix made scope, dependencies, and testing straightforward.

UML & Service Boundaries: Clear use-case diagrams, domain model (Student, Instructor, Admin, Lesson, Package, Payment, Vehicle, TestItem, Attempt), and sequence diagrams for scheduling, rescheduling, and taking a practice test. Logical 3-tier split (Web UI ↔ REST API ↔ DB) made responsibilities explicit.

Scheduling Model: Codified instructor availability + conflict checks (vehicle/instructor double-booking prevention), cancellation windows, and notification hooks.

Risk/Control Thinking: Identified top risks (no-shows, fraudulent chargebacks, data exposure) and aligned mitigations (reminder cadence, audit logs, least-privilege roles, input validation).

What I Would Revise (and How)

Quantify NFRs more tightly: Add explicit SLOs (e.g., p95 latency targets per endpoint, 99.9% for notification delivery within 2 min). Tie these to monitoring alerts.

Data Privacy & Retention: Add a data classification table, field-level encryption for sensitive data (e.g., contact numbers), and an automated retention policy (e.g., purge lesson location data after 180 days).

Test Strategy Depth: Expand from happy-path to property-based tests for scheduling edge cases (time zones, daylight savings, back-to-back lessons), plus load tests for registration spikes and security tests (OWASP ASVS mapping).

Error Handling UX: Standardize API error codes/messages and surface actionable UI states (retry, contact support, fallback calendars).

How I Interpreted User Needs (and Implemented Them)

Personas: Student (quick booking, clear pricing, fast practice tests), Instructor (predictable schedules, easy calendar management), Admin (control and insight).

Methods: Stakeholder interview notes → user stories (INVEST), MoSCoW prioritization, low-fi wireframes to validate flows, then UML + data model.

Examples:

“I need to book lessons around my school schedule” → calendar with availability filters, cancellation rules, SMS/email reminders.

“I want realistic exam prep” → tagged test bank (topic, difficulty), timed attempts, analytics on weak areas.

Why user-needs matter: They determine usability, adoption, and operational efficiency. Encoding them early reduced rework and made the SDD directly testable.

My Software Design Approach (Going Forward)

Process: Iterative, user-story driven; start with outcomes, not features. Validate with quick prototypes before deep build.

Techniques & Artifacts I’ll reuse:

Event storming to find domain events (LessonScheduled, LessonCanceled, AttemptSubmitted).

CRC cards to refine responsibilities and reduce coupling.

UML set: use cases, domain/class diagram, key sequence diagrams.

Architecture decision records (ADRs) for choices (e.g., PostgreSQL over MySQL, JWT sessions vs. server sessions).

Risk-first modeling: threat modeling (STRIDE), OWASP ASVS checklist.

Quality gates: CI with linting/tests, API contract tests (OpenAPI), seed data for repeatable demos, and basic observability (request IDs, structured logs, uptime checks).

Reference Architecture (conceptual):

Frontend: React SPA (role-aware routes, form validation).

Backend: RESTful API (e.g., Flask/FastAPI/Express), RBAC middleware, background jobs for reminders.

Data: PostgreSQL with normalized schema; caching (Redis) for schedule lookups; S3-style object store for static assets.

Security: TLS everywhere, hashed passwords, prepared statements/ORM, rate limiting, audit logging.
