# Aveli System Analysis Repository Rules

> **Methodology:** System-Structured Analysis Documentation (SSAD)  
> **Status:** Working methodology / project governance  
> **Scope:** This file defines how system analysis documentation is structured, written, linked, and reviewed in this repository.

---

## 1. Purpose

This repository is not organized around analyst artifact types.

It is organized around the **structure of the system itself**.

The documentation should feel familiar to developers, system analysts, architects, QA engineers, DevOps engineers, and other technical specialists because every major documentation area corresponds to a real responsibility, component, subsystem, technology, or operational concern of the product.

The primary rule is:

> **Documentation mirrors the system.**

A reader must be able to start from the whole system, move into a component, inspect its technology stack, understand its internal behavior, and then follow references to the exact places where that technology or component is used.

---

## 2. Working Methodology Name

The working name of this approach is:

# System-Structured Analysis Documentation (SSAD)

The name describes the core idea:

- **System-Structured** — the repository hierarchy follows the structure of the system;
- **Analysis** — the repository captures requirements, decisions, constraints, behavior, interfaces, data, and operational concerns;
- **Documentation** — the result is a maintained technical knowledge base rather than a one-time analytical deliverable.

The name is intentionally treated as a working name and may evolve as the methodology becomes more formalized.

This methodology may share individual practices with docs-as-code, architecture decision records, C4-style views, technical specifications, and developer documentation. SSAD does not claim that those practices are new. Its defining principle is the **combined system-oriented structure, explicit stack modeling, contextual usage documentation, bilingual documentation, and cross-project analytical potential**.

---

## 3. Core Principles

### 3.1 Documentation mirrors the system structure

Documentation hierarchy must be based primarily on **system responsibilities and technical layers**, not on the professional role that created the artifact.

Do not organize the repository only as:

```text
requirements/
uml/
architecture/
api/
security/
```

Prefer:

```text
business/
database/
backend/
frontend/
integrations/
operations/
system/
```

and then place requirements, diagrams, API contracts, schemas, decisions, and examples inside the component that owns them.

---

### 3.2 Mandatory system perspectives

Every project must explicitly describe the following perspectives:

```text
business/
database/
backend/
frontend/
```

If a perspective is genuinely not applicable, it must be explicitly documented as not applicable rather than silently omitted or artificially invented.

Projects may add other top-level areas when required by the actual architecture:

```text
integrations/
operations/
system/
result/
services/
plugins/
workers/
gateways/
```

The repository structure is allowed to differ between projects when the system itself differs.

---

### 3.3 Progressive depth

Documentation must support different levels of reading depth.

A reader should be able to follow:

```text
System
  ↓
Component
  ↓
Responsibility
  ↓
Technology
  ↓
Usage
  ↓
Implementation detail
```

A newcomer should not need to read the entire repository to understand the project.

Each major directory should be a valid entry point into its area.

---

### 3.4 Every significant directory is an entry point

Every meaningful documentation directory should contain:

```text
README.md
README.ru.md
```

The README should answer:

1. What is this area?
2. Why does it exist?
3. What responsibility does it own?
4. What is inside it?
5. Where should the reader go next?

A directory README is a navigation and context document, not a dump of all implementation details.

---

## 4. Language Rules

All explanatory project documentation must exist in two maintained versions:

```text
document.md
document.ru.md
```

For directory entry points:

```text
README.md
README.ru.md
```

The English version is the default repository language unless explicitly stated otherwise.

Both versions must describe the same system behavior and decisions.

The following artifacts are language-neutral and normally exist only once:

- SQL;
- OpenAPI specifications;
- JSON / YAML schemas;
- source code examples where translation is meaningless;
- machine-readable metadata;
- PlantUML / Mermaid source where labels are language-neutral;
- database DDL;
- configuration examples.

A translation must preserve meaning rather than mechanically translate terminology.

Technology names, API paths, identifiers, database fields, class names, event names, and other implementation identifiers must not be translated.

---

## 5. Business Documentation Boundary

The `business/` area describes the product from the perspective of goals, scope, behavior, requirements, constraints, business rules, workflows, and acceptance.

Typical structure:

```text
business/
├── README.md
├── README.ru.md
├── context/
├── scope/
├── requirements/
├── processes/
└── traceability/
```

Business documentation answers questions such as:

- Why does the product exist?
- Who uses it?
- What does the system do?
- What is in scope?
- What is out of scope?
- What behavior is required?
- What business rules must remain true?
- What result is considered acceptable?

### 5.1 Technical leakage is not allowed

Business documentation must not become a mixture of business and implementation details.

Avoid implementation statements such as:

```text
Stored in PostgreSQL
Implemented with NestJS
Uses Redis
Uses JWT access + refresh tokens
Runs in Docker
```

when the business meaning can be expressed without them.

Prefer:

```text
Account state is server-managed.
Operational workspace data is not synchronized between devices.
Access is controlled at workspace level.
Subscription state affects workspace availability.
```

Then reference the technical documentation:

```text
See: ../../backend/
See: ../../database/
See: ../../integrations/
```

### 5.2 Product-significant technical constraints

A technical fact may appear in business documentation only when it materially changes product behavior, scope, contractual expectations, regulatory constraints, or user-visible capabilities.

Even then, detailed implementation belongs in the technical area.

---

## 6. Database Documentation

The `database/` area represents the complete data architecture of the project.

It must make it possible to understand both:

1. the conceptual and logical data model;
2. the real physical storage model.

Typical structure:

```text
database/
├── README.md
├── README.ru.md
├── architecture/
├── stack/
├── local/
├── server/
└── shared/
```

Only applicable subdirectories should be created.

Database documentation may include:

- data ownership;
- logical models;
- physical models;
- entities;
- relationships;
- primary and foreign keys;
- constraints;
- indexes;
- enums;
- nullability;
- timestamps;
- money representation;
- time representation;
- lifecycle rules;
- migration rules;
- backup and restore;
- retention;
- real SQL DDL;
- example queries;
- schema references.

### 6.1 Real artifacts are preferred

When possible, documentation must include or reference real artifacts:

```text
schema.sql
*.prisma
Drift table definitions
migration files
ER diagrams
sample queries
```

The repository should not stop at conceptual entity lists when the physical implementation is known.

---

## 7. Backend Documentation

The `backend/` area describes all backend responsibilities.

Typical structure:

```text
backend/
├── README.md
├── README.ru.md
├── architecture/
├── stack/
├── api/
├── auth/
├── services/
├── access/
├── billing/
├── messaging/
├── cache/
├── logs/
├── errors/
├── security/
└── deployment/
```

Only applicable areas should exist.

Backend documentation may include:

- runtime architecture;
- backend technology stack;
- REST or other API style;
- API contracts;
- authentication;
- authorization;
- session lifecycle;
- domain services;
- background jobs;
- messaging;
- caching;
- logging;
- error handling;
- retries;
- idempotency;
- secrets;
- containerization;
- deployment;
- configuration;
- code examples where useful.

---

## 8. Frontend Documentation

The `frontend/` area describes the client application and its responsibilities.

Typical structure:

```text
frontend/
├── README.md
├── README.ru.md
├── architecture/
├── stack/
├── bootstrap/
├── navigation/
├── state/
├── workspace/
├── storage/
├── offline/
├── notifications/
├── errors/
└── localization/
```

Frontend documentation should explain:

- client architecture;
- selected framework and libraries;
- state management;
- navigation;
- local persistence usage;
- API consumption;
- startup and bootstrap;
- authentication state handling;
- offline behavior;
- UI state transitions;
- notification lifecycle;
- error recovery;
- localization;
- platform-specific constraints.

Frontend documentation should not duplicate backend API definitions. It should reference the canonical backend API contract and document only frontend-specific consumption behavior.

---

## 9. Stack Is a First-Class Analytical Object

Every major technical layer must explicitly document its stack.

Examples:

```text
backend/stack/
frontend/stack/
database/stack/
operations/stack/
```

A stack directory is not a list of badges.

Every important technology should be documented as an analytical object.

Example:

```text
backend/stack/kafka/
backend/stack/redis/
backend/stack/rest-api/
backend/stack/docker/
```

A technology document should answer, where applicable:

- What is the technology?
- Why is it used in this project?
- Where is it used?
- What responsibility does it support?
- Which components depend on it?
- Is it critical or replaceable?
- What alternatives were considered?
- What limitations does it introduce?
- What operational cost does it introduce?
- Which requirements or NFRs does it support?
- Where is its contextual usage documented?

### 9.1 Stack before usage

The general technology decision is documented first in `stack/`.

Its concrete usage is then documented in the component where it participates.

Example:

```text
backend/stack/kafka/
```

describes Kafka as a backend technology decision.

```text
backend/logs/kafka-usage.md
```

describes how Kafka is used specifically by logging.

These documents must not duplicate each other.

---

## 10. Canonical Knowledge and Contextual Usage

A system may use the same technology or concept in several places.

Documentation must distinguish:

### Canonical knowledge

The single source of truth for the general concept, technology, contract, or model.

Example:

```text
backend/stack/kafka/
```

### Contextual usage

The way that concept is used in a specific component.

Example:

```text
backend/logs/
backend/audit/
backend/notifications/
```

Contextual documents should reference the canonical document and contain only usage-specific information.

Rule:

> **Do not duplicate knowledge. Duplicate context when necessary.**

This prevents documentation drift.

---

## 11. Interfaces and API Contracts

Interfaces belong to the component that owns the contract.

For example:

```text
backend/api/
```

is the canonical source for backend HTTP API contracts.

It may contain:

```text
openapi.yaml
error-model.md
auth/
access/
billing/
webhooks/
```

The stack-level documentation may describe REST as an architectural choice:

```text
backend/stack/rest-api/
```

but it must not duplicate concrete endpoint contracts.

Other components reference the canonical interface.

Example:

```text
frontend/auth/
  → references backend/api/auth/

integrations/revenuecat/
  → references backend/api/webhooks/
```

---

## 12. Integrations and External Systems

External integrations must be documented separately when they have meaningful behavior, contracts, state, configuration, security, or failure scenarios.

Typical structure:

```text
integrations/
├── README.md
├── README.ru.md
├── revenuecat/
├── google-play/
├── app-store/
├── device-contacts/
└── exchange-rates/
```

An integration may document:

- role in the system;
- ownership boundary;
- API or SDK;
- authentication;
- webhooks;
- external entities;
- internal mapping;
- failure scenarios;
- retries;
- idempotency;
- security;
- configuration;
- operational dependency;
- related frontend/backend components.

If an integration evolves into an independent subsystem with its own data model, API, runtime, and lifecycle, it may become a top-level system component.

---

## 13. Additional System Components

The methodology is not limited to Business / Database / Backend / Frontend.

A real system may contain additional major components such as:

```text
sync-service/
integration-gateway/
revit-plugin/
worker/
ml-service/
file-service/
analytics-service/
```

A component deserves its own top-level area when it has a meaningful independent combination of:

- responsibility;
- internal architecture;
- data;
- API or interfaces;
- technology stack;
- lifecycle;
- deployment;
- operational ownership.

Do not create top-level directories for technologies merely because they are important.

For example, Kafka, Redis, Docker, React, Flutter, and Three.js normally belong inside the stack or usage context of the component that owns them.

---

## 14. System-Level View

The `system/` area provides views that cross component boundaries.

Typical structure:

```text
system/
├── architecture/
├── stack/
├── decisions/
└── dependencies/
```

This area may contain:

- system context;
- component map;
- high-level data flows;
- integration sequences;
- technology map;
- dependency graph;
- architecture decision records;
- cross-layer constraints.

The `system/` area must summarize and connect component documentation rather than re-document every component.

---

## 15. Architecture Decisions

Important architectural decisions should be explicitly recorded.

Recommended format:

```text
system/decisions/
├── ADR-001-*.md
├── ADR-002-*.md
└── ...
```

An ADR should normally contain:

- context;
- problem;
- decision;
- rationale;
- alternatives considered;
- consequences;
- affected components;
- related requirements;
- status.

Architectural decisions are valuable not only for current understanding but also for future cross-project analysis.

---

## 16. Cross-References Form the Knowledge Graph

The file hierarchy is a navigation tree.

The actual system knowledge is a graph.

Documents must link to related documents whenever the relationship is meaningful.

Examples:

```text
business requirement
    → backend capability

backend API
    → frontend consumer

database entity
    → backend service

RevenueCat integration
    → billing service
    → access model

technology
    → component usage
    → requirement
```

Rule:

> **Storage is hierarchical. Knowledge is graph-based.**

Cross-references should be intentional and should help a reader move between abstraction levels.

---

## 17. Document Structure and Readability

Documentation must be written as readable technical prose, not as a raw list of facts.

A document should normally progress from broad context to deeper detail.

Recommended pattern:

```text
# Title

## Overview
## Responsibility
## Boundaries
## Model / Structure
## Behavior
## Data / Interfaces
## Constraints
## Failure Scenarios
## Operations
## Related Documentation
## References
```

Not every document needs every section.

The structure must match the document's purpose.

### 17.1 Lists are supporting elements

Lists and tables are encouraged where they improve scanning.

They must not replace explanation when context is required.

For example, a list of Core Functional Areas should explain what each area means rather than only naming the areas.

---

## 18. Diagrams

Diagrams must support the text, not replace it.

Use diagrams when they clarify:

- system context;
- component relationships;
- sequence behavior;
- state transitions;
- data models;
- deployment;
- integration;
- technology usage;
- dependency structure.

Diagram source should be stored in the repository whenever possible.

Preferred machine-maintainable formats include:

```text
PlantUML
Mermaid
Graphviz
```

Rendered assets may be stored separately for convenient viewing.

Every diagram should have enough surrounding text to explain what the reader is looking at and why it matters.

---

## 19. Code and Machine-Readable Artifacts

Technical documentation may include real or representative implementation artifacts when they improve understanding.

Examples:

```text
SQL
OpenAPI
JSON
YAML
Dockerfile
docker-compose
configuration examples
DTO examples
HTTP examples
migration examples
code snippets
```

Examples should be clearly marked as one of:

- real implementation;
- simplified implementation;
- illustrative example.

Do not present illustrative code as if it were the actual implementation.

---

## 20. Operations and Lifecycle

When relevant, the repository must describe how the system is operated after development.

Possible areas include:

```text
operations/
├── deployment/
├── configuration/
├── observability/
├── logs/
├── metrics/
├── tracing/
├── backups/
├── migrations/
├── incident-recovery/
├── release/
└── testing/
```

Operational documentation should explain not only how to start the system, but how it behaves over time.

---

## 21. Traceability

Important system behavior should be traceable across abstraction levels.

Target traceability model:

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
Acceptance Criterion
    ↓
Test / Verification
```

Not every low-level detail requires a full traceability chain.

Traceability should focus on meaningful system decisions and high-impact requirements.

---

## 22. Analytical Metadata

The methodology is designed to support future automated analysis across projects.

Important technologies, components, decisions, and relationships may later include machine-readable metadata.

Example:

```yaml
technology: kafka
layer: backend
category: messaging

used_by:
  - logs
  - audit

supports:
  - NFR-012

criticality: medium
replaceable: true

alternatives:
  - rabbitmq
  - direct-http

decision_status: accepted
```

Metadata should not replace human-readable documentation.

Its purpose is to make the repository analyzable by tooling and AI.

Possible future analyses include:

- technology frequency across projects;
- technology criticality;
- architecture decision comparison;
- unused or overused infrastructure;
- requirement-to-technology mapping;
- component dependency analysis;
- recurring failure patterns;
- stack evolution;
- operational complexity;
- documentation completeness.

---

## 23. AI Review Rules

AI-based repository review should evaluate documentation against these rules.

An AI reviewer should check for:

### Structure

- Are the major system perspectives documented?
- Does the hierarchy reflect the actual system?
- Are technologies placed under the components that own them?
- Are independent subsystems separated when justified?

### Boundaries

- Does business documentation avoid unnecessary implementation detail?
- Does backend documentation avoid duplicating frontend behavior?
- Does frontend documentation reference canonical API contracts?
- Are integration boundaries explicit?

### Completeness

- Are important components documented?
- Is stack documentation present?
- Are entities and interfaces described?
- Are failure scenarios covered where relevant?
- Are operational concerns documented where applicable?

### Consistency

- Do English and Russian documents describe the same behavior?
- Do cross-references resolve correctly?
- Is canonical knowledge duplicated elsewhere?
- Do diagrams contradict prose?
- Do examples contradict actual contracts or schemas?

### Traceability

- Can important requirements be connected to implementation decisions?
- Can major technologies be connected to concrete usages?
- Can major architectural decisions be connected to their rationale?

### Readability

- Does each document explain its subject rather than only list facts?
- Can a newcomer understand the area without reading unrelated directories?
- Is the level of detail appropriate for the document's scope?

---

## 24. What This Methodology Is Not

SSAD is not intended to be:

- a rigid folder template for every system;
- a replacement for UML, BPMN, C4, OpenAPI, ADR, SQL, or other established notations;
- a requirement that every project use every possible documentation artifact;
- a reason to document technologies that are not actually used;
- a reason to duplicate the same information across multiple directories;
- a substitute for source code;
- a substitute for product management;
- a substitute for operational runbooks where dedicated runbooks are required.

The methodology defines **how knowledge about the system is organized and connected**.

It does not prescribe one architecture or one implementation stack.

---

## 25. Repository Quality Standard

A repository following this methodology should allow a new technical specialist to answer, with minimal searching:

```text
What does this system do?
What is in scope?
What are its main components?
What data exists?
Who owns that data?
What technologies are used?
Why were they selected?
Where are they used?
How do components communicate?
What are the important constraints?
What happens when something fails?
How is the system operated?
Where is the canonical source for this information?
```

The reader chooses how deep to go.

The repository must support that choice.

---

## 26. Core Summary

The methodology can be reduced to the following rules:

```text
Documentation mirrors the system.

Business explains what and why.
Technical layers explain how.

Stack is explicit.
Usage is contextual.

Canonical knowledge has one source of truth.
Related knowledge is connected by references.

The hierarchy is a tree.
The knowledge model is a graph.

Documentation supports progressive depth.

Human-readable documentation is bilingual.
Machine-readable artifacts remain language-neutral.

Real technical artifacts are preferred over abstract descriptions.

The repository should be useful both to people
and to automated analytical tools.
```

---

## 27. Current Project Baseline: Aveli

For Aveli, the expected high-level documentation model is:

```text
business/
database/
backend/
frontend/
integrations/
operations/
system/
result/
```

This baseline may evolve as the methodology and the system itself evolve.

Changes to the methodology should be reflected in this file before large-scale repository restructuring.
