# System-Structured Analysis Documentation

> **Working name:** System-Structured Analysis Documentation (SSAD)  
> **Status:** Evolving methodology  
> **Purpose:** Describe the reasoning, structure, and philosophy behind this repository approach.

---

## What This Document Is

`methodology.md` explains **the idea behind the documentation approach**.

It is not a rulebook.

The repository rules are defined separately in:

[`rules.md`](rules.md)

The distinction is:

```text
methodology.md
    ↓
Why the approach exists
How it is intended to work
What problem it solves
How its parts relate

rules.md
    ↓
What repository documents must follow
What is allowed
What is not allowed
How consistency is checked
```

The methodology may evolve as the repository grows and new projects expose weaknesses or missing concepts.

---

## Core Idea

The central idea is simple:

> **System analysis documentation should mirror the structure of the system being analyzed.**

Traditional analytical repositories are often organized by artifact type:

```text
requirements/
uml/
api/
database/
security/
```

This is useful for the author, but it does not always reflect how the system itself is understood by the people who build and maintain it.

SSAD instead organizes knowledge around system responsibilities:

```text
business/
database/
backend/
frontend/
integrations/
operations/
system/
```

The exact structure is not fixed.

The hierarchy should follow the real system.

---

## Why This Approach Exists

A system is not experienced as a collection of analyst artifacts.

A backend developer thinks in services, APIs, authentication, billing, messaging, errors, and infrastructure.

A frontend developer thinks in screens, state, navigation, local storage, offline behavior, and integrations.

A database specialist thinks in data ownership, schemas, entities, constraints, migrations, and lifecycle.

A system analyst must understand all of these perspectives.

Therefore the documentation should allow every participant to enter the repository through the part of the system that is relevant to them, while still being able to navigate toward the full picture.

---

## Documentation as a System Model

The repository itself becomes a model of the system.

At the highest level:

```text
System
  ↓
Major responsibility
  ↓
Component
  ↓
Technology
  ↓
Behavior
  ↓
Implementation detail
```

A reader chooses how deep to go.

This creates **progressive depth**.

A newcomer may only need:

```text
business/
system/
```

A backend developer may continue into:

```text
backend/
backend/api/
backend/auth/
backend/stack/
```

A database specialist may enter directly through:

```text
database/
```

The repository should support all of these reading paths without requiring one mandatory linear document sequence.

---

## Hierarchy and Knowledge Graph

The repository has two simultaneous structures.

### Physical Structure

Files and folders form a hierarchy.

```text
backend/
└── auth/
    ├── README.md
    ├── sessions.md
    └── token-lifecycle.md
```

This is useful for navigation and ownership.

### Knowledge Structure

Real system knowledge is not hierarchical.

Authentication may depend on:

```text
business requirements
database session entities
backend auth logic
frontend bootstrap
security constraints
architecture decisions
```

Therefore documents must cross-reference related knowledge.

The core principle is:

> **Storage is hierarchical. Knowledge is graph-based.**

The directory tree helps the reader find information.

Cross-references explain how the system actually works.

---

## Canonical Knowledge

Every important concept should have one canonical location.

Examples:

```text
backend/api/
    → canonical API contracts

database/
    → canonical data model

backend/stack/kafka/
    → canonical explanation of Kafka usage in the backend

business/requirements/
    → canonical business requirements
```

Other documents reference the canonical source instead of repeating it.

This creates another core principle:

> **Do not duplicate knowledge. Duplicate context when necessary.**

For example:

```text
backend/stack/kafka/
```

may explain:

- why Kafka is used;
- what role it has in the backend;
- what alternatives exist;
- what dependencies it creates.

Then:

```text
backend/logs/
```

may explain:

- how logging uses Kafka;
- which events are published;
- which failure scenarios matter.

The second document does not redefine Kafka.

It describes Kafka in the context of logging.

---

## Stack as an Analytical Object

Technology choices are not treated as incidental implementation details.

The stack is a first-class analytical dimension.

A technology should answer more than:

```text
"We use Redis."
```

Documentation should eventually make it possible to understand:

```text
Why Redis?
Where is it used?
Which components depend on it?
Which requirement does it support?
What breaks if it is removed?
Could it be replaced?
What alternatives were considered?
What operational cost does it introduce?
```

This is why stack documentation exists explicitly inside technical areas.

Example:

```text
backend/
└── stack/
    ├── redis/
    ├── kafka/
    ├── rest-api/
    └── docker/
```

The technology then appears again only through contextual references where it is actually used.

---

## Three Dimensions of Technical Knowledge

SSAD treats technical knowledge as three connected dimensions.

### 1. Structural Knowledge

What exists in the system?

```text
backend
auth
billing
logs
notifications
database
frontend
```

### 2. Technology Knowledge

What technologies are used?

```text
NestJS
PostgreSQL
Redis
Kafka
Flutter
Drift
Docker
```

### 3. Usage Knowledge

Where and why does each technology participate?

```text
Kafka
  → logs
  → audit
  → notifications

Redis
  → cache
  → session support

Drift
  → local workspace persistence
```

This model is important for both human understanding and future automated analysis.

---

## Business and Technical Boundaries

Business documentation and technical documentation must remain connected but separate.

Business answers:

```text
Why does this exist?
What does the user need?
What is in scope?
What behavior must remain true?
What result is acceptable?
```

Technical documentation answers:

```text
How is it implemented?
Which component owns it?
Which technology supports it?
Which data is used?
Which interfaces are involved?
How does it fail?
How is it operated?
```

A business statement may have technical consequences.

Example:

```text
Business:
Professional workspace data is not synchronized between devices.

Technical consequences:
→ frontend/storage/
→ database/local/
→ system/decisions/
```

The business document defines the product truth.

The technical documents define how the system preserves that truth.

---

## Business Is Not a Technical Summary

A business document should not become a shortened version of the architecture.

For example:

```text
Stored in PostgreSQL
Uses JWT
Implemented with NestJS
```

does not belong in business documentation unless the technology itself has a direct product-level consequence.

Prefer:

```text
Account state is server-managed.
Access is controlled at workspace level.
Professional data is not synchronized between devices.
```

Then link to the technical implementation.

This keeps business documentation readable while preserving traceability.

---

## Technical Areas Are Not Artifact Buckets

A technical directory is not simply a place to store every document of a certain format.

For example:

```text
backend/api/
```

owns actual API contracts.

```text
backend/stack/rest-api/
```

owns REST as an architectural and technological decision.

These are related but not interchangeable.

Similarly:

```text
database/
```

owns data architecture.

It is not merely a folder containing ER diagrams.

It may include:

```text
architecture
stack
entities
schemas
constraints
indexes
migrations
queries
ownership
```

The organizing principle is responsibility, not file format.

---

## Cross-Cutting Views

Some knowledge does not belong to one component.

Examples:

```text
system architecture
technology map
dependency graph
architecture decisions
traceability
delivery changes
```

These are cross-cutting views.

They should connect component-level knowledge rather than duplicate it.

For example:

```text
system/
```

can show how backend, frontend, database, and integrations form one system.

It should not become a second copy of all component documentation.

---

## Traceability as Navigation Between Abstraction Levels

Traceability is not only:

```text
Requirement → Test
```

In SSAD, the goal is to connect several levels:

```text
Business Need
    ↓
Business Rule
    ↓
Requirement
    ↓
Architecture Decision
    ↓
Component
    ↓
Technology / Interface / Data
    ↓
Acceptance
    ↓
Verification
```

Not every trace needs every step.

The important part is that significant decisions can be followed both forward and backward.

### Forward

```text
Why does the system need this?
    ↓
How is it implemented?
```

### Backward

```text
Why does this technology or component exist?
    ↓
Which product need justifies it?
```

The second direction is especially valuable when reviewing architecture.

---

## Tasks and Change Artifacts

A task is not inherently a business artifact.

Tasks can exist in any area:

```text
backend
frontend
database
operations
integration
```

A task belongs to the context of the change it describes.

Therefore task directories should not be created automatically inside every layer.

They appear only when the project actually needs task-level documentation.

At the business level, scope, requirements, business rules, processes, and acceptance criteria already define what the product must achieve.

Creating an additional generic `business/tasks/` layer would usually duplicate that intent.

---

## Documentation Should Support Real Implementation

SSAD is intended to move beyond abstract analytical descriptions.

Where appropriate, technical documentation should include or reference real artifacts:

```text
OpenAPI
SQL
DDL
Prisma schema
Drift tables
JSON examples
configuration examples
Docker files
migration scripts
code fragments
PlantUML
```

The goal is not to turn documentation into source code.

The goal is to reduce the gap between analytical intent and the actual system.

---

## Documentation Is for Multiple Roles

The same repository should be useful to different readers.

### Product or Business Reader

```text
business/
```

### System Analyst

```text
business/
system/
database/
backend/
frontend/
integrations/
```

### Backend Developer

```text
backend/
database/
integrations/
```

### Frontend Developer

```text
frontend/
backend/api/
database/local/
```

### DevOps / Operations

```text
operations/
system/
backend/deployment/
```

### QA

```text
business/requirements/
business/traceability/
operations/testing/
technical component behavior
```

The methodology does not hide unrelated information from a role.

It provides a clear entry point and lets the reader move deeper when necessary.

---

## Human-Readable and Machine-Readable Knowledge

The repository is designed for both people and automated analysis.

Human-readable documentation explains meaning and context.

Machine-readable metadata may later describe relationships.

Example:

```yaml
technology: redis
layer: backend
category: cache

used_by:
  - sessions
  - access

supports:
  - NFR-036

criticality: medium
replaceable: true
```

This can eventually allow automated questions such as:

```text
Where is Redis used?
Which projects depend on Kafka?
Which technologies repeatedly create operational complexity?
Which requirements have no acceptance criteria?
Which components have no documented failure scenarios?
Which architectural decisions repeat across projects?
```

The methodology is intentionally designed so that AI tools can analyze repository structure without replacing human-readable documentation.

---

## Multi-Project Analysis

One long-term goal is to make documentation comparable across projects.

If several repositories use the same conceptual model, they can later be analyzed together.

For example:

```text
Project A uses Kafka for logs and audit.
Project B uses Kafka only for notifications.
Project C uses no broker.

Why?

What requirements justified the choice?

Was the operational cost worth it?
```

The methodology therefore treats architecture decisions, technology usage, ownership, criticality, and traceability as potentially analyzable information.

This is not required for the first version of a project.

It is a direction the structure should make possible.

---

## Bilingual Documentation

Human-readable project documentation is maintained in English and Russian.

The goal is not two independent documentation sets.

They represent the same knowledge in two languages.

Machine-readable artifacts remain shared where translation would create meaningless duplication.

Example:

```text
README.md
README.ru.md

openapi.yaml       ← shared
schema.sql         ← shared
dependencies.yaml  ← shared
```

---

## What SSAD Is Not

SSAD is not intended to replace:

- UML;
- BPMN;
- C4;
- OpenAPI;
- ADR;
- SQL documentation;
- source code;
- testing documentation;
- operational runbooks;
- product management.

It is a way to **organize and connect those forms of knowledge around the system itself**.

It also does not require every project to have identical directories.

A Revit plugin, a mobile application, and a distributed banking platform should not have the same physical documentation tree if their architectures are different.

---

## Working Definition

The current working definition is:

> **System-Structured Analysis Documentation is a documentation methodology in which analytical and technical knowledge is organized around the actual structure and responsibilities of a system, while cross-references connect business intent, architecture, technologies, implementation context, and verification into a navigable knowledge graph.**

The methodology favors:

```text
system-oriented hierarchy
+
progressive depth
+
canonical knowledge
+
contextual usage
+
explicit technology modeling
+
traceability
+
real technical artifacts
+
human and machine readability
```

---

## Evolution

This methodology is intentionally not considered final.

New projects may reveal:

- missing abstraction levels;
- weak boundaries;
- unnecessary duplication;
- better naming;
- better metadata models;
- new cross-project analytical possibilities.

When that happens:

```text
Observation
    ↓
Methodology change
    ↓
Rule update if necessary
    ↓
Repository structure evolves
```

The methodology should grow from real project experience rather than from theoretical completeness.
