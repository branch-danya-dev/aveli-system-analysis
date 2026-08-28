# Aveli System Analysis Repository Rules

> **Methodology:** System-Structured Analysis Documentation (SSAD)  
> **Status:** Repository governance baseline  
> **Scope:** Normative rules for structuring, writing, linking and reviewing documentation in this repository.

[Русская версия](rules.ru.md) · [Methodology](methodology.md)

---

## 1. Primary Rule

> **Documentation mirrors the system.**

Repository structure MUST follow actual responsibilities, boundaries and ownership before it follows analyst artifact categories.

Do not create directories only because a generic methodology example contains them. Create an area when the system has real knowledge that needs an owner.

---

## 2. Required Analytical Perspectives

Every analyzed system MUST make the following questions explicit:

```text
What problem/product exists?
What is in and out of scope?
What behavior is required?
What data exists and who owns it?
What runtime components own responsibilities?
How do components communicate?
What crosses the external system boundary?
Which technologies support which responsibilities?
What trust/failure/security constraints exist?
How is important behavior accepted or verified?
How do the components form one system?
```

These are mandatory **perspectives**, not mandatory folder names.

For Aveli, the current top-level analytical structure is:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

`operations/`, `worker/`, `plugin/`, `gateway/`, or other top-level areas are added only when real architecture justifies them.

---

## 3. Significant Directories Are Entry Points

Every meaningful human-readable documentation directory SHOULD contain:

```text
README.md
README.ru.md
```

Its README SHOULD explain purpose, responsibility, boundary, important contents, and where to read next.

A README is navigation/context, not a duplicate of all child documents.

---

## 4. Language Rules

Human-readable project documentation MUST be maintained as equivalent English/Russian pairs when the area participates in the bilingual repository:

```text
document.md
document.ru.md
```

Directory entry points:

```text
README.md
README.ru.md
```

Both versions MUST describe the same behavior and decisions.

Machine-readable or language-neutral artifacts normally exist once:

```text
OpenAPI
SQL / DDL
JSON / YAML schemas
PlantUML / Mermaid with neutral labels
configuration examples
source identifiers
```

API paths, class names, database fields, configuration keys and other implementation identifiers MUST NOT be translated.

---

## 5. Business Boundary

`business/` owns product intent:

```text
context
scope
requirements
business rules
processes
acceptance
traceability
```

Business documents MUST prefer product meaning over implementation detail.

Prefer:

```text
Account state is server-managed.
Professional workspace data is not synchronized between devices.
Access is controlled at workspace level.
```

Avoid technology names unless they materially change product behavior, scope, an external contract, regulation, or a user-visible capability.

Technical consequences MUST be linked to their owning technical area.

---

## 6. Canonical Ownership

Every important concept MUST have one canonical owner.

Before writing detail, identify:

```text
Who decides?
Who stores canonical state?
Who may change it?
Who consumes it?
Who verifies it?
```

Other documents MAY repeat concise context for readability, but MUST link to the canonical source when the fact is material.

> **Do not duplicate knowledge. Duplicate context when necessary.**

If two documents can independently drift into contradictory definitions of the same fact, ownership is not sufficiently clear.

---

## 7. Data Documentation

`database/` owns system data architecture and physical persistence knowledge.

Data modeling SHOULD progress:

```text
ownership
→ conceptual model
→ logical model
→ physical model
→ constraints / migrations / lifecycle
```

System-wide conceptual/logical views MUST NOT erase physical storage boundaries.

Physical models belong to the persistence owner that stores them.

For Aveli:

```text
database/local/
→ SQLite workspace + files + persistence lifecycle

database/server/
→ PostgreSQL identity/access persistence
```

Frontend/backend documents describe how they use persistence; they MUST NOT redefine canonical schemas.

---

## 8. Runtime Component Documentation

A component area such as `backend/` or `frontend/` owns behavior for that runtime component.

Component documentation SHOULD cover applicable concerns such as:

```text
responsibility boundary
architecture
state / behavior
interfaces
data usage
errors / failures
security
configuration
testing
technology stack
```

Only applicable subdirectories SHOULD exist. Do not create empty capability folders in anticipation of future implementation.

---

## 9. Technology Ownership and Stack

Technology is a first-class analytical object when it materially affects architecture.

Canonical technology ownership MUST follow the system responsibility the technology primarily realizes.

Aveli target ownership:

```text
SQLite
PostgreSQL
→ database/stack/

Drift
Flutter
Riverpod
go_router
client HTTP
secure storage
mobile SDKs
→ frontend/stack/

NestJS
Prisma
REST style
JWT
Argon2id
→ backend/stack/

RevenueCat provider boundary
→ integrations/revenuecat/
```

Contextual usage MAY be documented elsewhere, but canonical technology knowledge MUST NOT be duplicated across perspectives.

A significant technology document SHOULD answer, where known:

- role;
- why it is used;
- where it is used;
- main consumers/dependencies;
- limitations;
- criticality;
- replaceability;
- relevant alternatives;
- links to contextual usage.

---

## 10. API and Internal Interfaces

A concrete contract belongs to the component that owns the interface.

For Aveli:

```text
backend/api/
→ canonical backend HTTP contracts
```

`backend/stack/rest-api/` MAY explain REST as an architectural/technology style but MUST NOT duplicate endpoint contracts.

Frontend documentation MUST reference backend API contracts and describe only client-specific consumption, retry, state or UI behavior.

The internal:

```text
Frontend ↔ Backend
```

boundary is NOT documented as an external integration.

---

## 11. External Integrations

`integrations/` owns boundaries that cross outside the analyzed system.

An integration deserves a canonical area when it has meaningful:

```text
external responsibility
contract / SDK
identity mapping
data crossing the boundary
authentication / trust
configuration
failure semantics
retry / idempotency / reconciliation
platform constraints
```

Small OS/package usage MAY remain contextual if it does not justify a standalone integration owner.

Provider state MUST NOT silently become Aveli product authority when the Aveli system owns a separate decision.

---

## 12. System Synthesis

`system/` owns cross-component knowledge.

It MAY contain:

```text
system context
component model
cross-layer boundaries
end-to-end flows
data movement
trust / authority model
system invariants
boundary-changing evolution
cross-system failure scenarios
release readiness
open questions
```

It MUST NOT re-document every component in full.

> **A layer may reference another layer, but a model that describes several layers together belongs to their nearest common system-level owner.**

The final system view SHOULD be synthesized after the relevant component views are sufficiently stable.

---

## 13. Tasks, Changes and Optional Areas

A task belongs to the area of change it describes. It is NOT inherently a business artifact.

Do NOT create `business/tasks/` or task directories in every component by default.

Likewise, do NOT create optional top-level areas such as `operations/` until real artifacts justify independent ownership.

If deployment, observability, backups, incident recovery or release operations become substantial, `operations/` MAY be introduced.

---

## 14. Traceability

High-impact product behavior SHOULD be traceable across abstraction levels.

Useful chain:

```text
Business Need
    ↓
Business Rule / Requirement
    ↓
Component / Decision
    ↓
Interface / Data / Technology
    ↓
Acceptance
    ↓
Verification
```

Not every trace requires every step.

Traceability SHOULD allow both:

```text
Why is this implemented?
```

and:

```text
How does this requirement survive implementation?
```

For Aveli, business traceability remains canonical in `business/traceability/`.

---

## 15. Evidence and Truth Status

Documentation of an existing system MUST prefer current evidence over architectural preference.

Relevant evidence includes:

```text
source code
schemas
API contracts
configuration
native project files
provider contracts
tests
runtime behavior
stakeholder decisions
```

When uncertainty matters, distinguish:

```text
VERIFIED
INFERRED
OPEN
```

A proposal MUST be marked as target/proposed state rather than mixed with current behavior.

Open questions SHOULD be explicit rather than silently resolved by assumption.

---

## 16. Maturity

Documents MAY use the conceptual maturity lifecycle:

```text
Draft
→ Baseline
→ Stable
```

`Stable` SHOULD mean the artifact has been cross-checked against the relevant implementation, contract, or stakeholder decision.

Do not label a document Stable merely because it is detailed.

---

## 17. Diagrams

Diagrams MUST support text, not replace it.

Use diagrams when they clarify system context, component relationships, sequences, state transitions, data models, integration boundaries or dependencies.

Machine-maintainable source SHOULD be kept when possible.

A diagram MUST NOT contradict canonical prose/contracts.

Rendered assets MAY be stored separately for fast review.

---

## 18. Real Technical Artifacts

When useful and available, technical documentation SHOULD reference real artifacts:

```text
OpenAPI
SQL / DDL
Prisma schema
Drift tables
JSON examples
configuration
migrations
code fragments
PlantUML
```

Examples MUST be distinguishable as:

```text
real implementation
simplified implementation
illustrative example
```

Do not present illustrative code as verified source.

---

## 19. Failure, Security and Release Views

Failure/security/release knowledge belongs to the nearest useful owner.

Component-local behavior stays with the component. Cross-system behavior MAY be synthesized under `system/`.

For Aveli:

```text
frontend/security/
backend/security/
integrations/
system/trust/
system/review/failure-scenarios.*
system/review/release-readiness.*
```

An independent `operations/` area is not required until operational ownership becomes substantial.

---

## 20. Legacy Migration and Refactoring

Before deleting or restructuring old documentation:

1. establish the new canonical owner;
2. check old files for unique/orphan knowledge;
3. migrate the unique knowledge;
4. fix cross-references;
5. remove duplicates;
6. re-run consistency review.

Never delete a legacy branch merely because a newer directory exists. The absence of orphan knowledge must be established first.

---

## 21. AI / Automated Review

Automated review SHOULD check:

### Structure

- Does the tree reflect the actual system?
- Are important perspectives explicit?
- Are optional directories justified?

### Ownership

- Does each important fact have one canonical owner?
- Are technologies owned by the responsibility they primarily realize?
- Are internal interfaces separated from external integrations?

### Consistency

- Do EN/RU documents agree?
- Do links resolve?
- Do diagrams match prose?
- Do examples match current contracts/schemas?
- Is duplicate canonical knowledge present?

### Traceability

- Can high-impact requirements be followed into implementation?
- Can significant technologies be connected to concrete usage?
- Are important OPEN questions visible?

### Readability

- Can a new reader understand an area from its README?
- Does the document explain meaning rather than only list facts?

AI MAY assist review but MUST NOT turn unsupported inference into canonical truth.

---

## 22. Current Aveli Baseline

Aveli currently uses:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

Supporting content:

```text
screenshots/
renderer/
README.md
README.ru.md
methodology.md
methodology.ru.md
rules.md
rules.ru.md
```

There is currently no standalone `operations/` or `result/` analytical perspective because the available knowledge does not justify those top-level owners.

If the architecture changes, the structure MAY change with it.

---

## 23. Repository Quality Gate

A mature repository SHOULD allow a new technical reader to answer with minimal searching:

```text
What does the system do?
What is in/out of scope?
What are the main components?
What data exists?
Who owns that data?
How do components communicate?
What external systems participate?
Which technologies are used and why?
Where are those technologies used?
What is authoritative for important decisions?
What happens when dependencies fail?
How is important behavior verified?
What questions are still open?
Where is the canonical source for each answer?
```

If these questions require reading unrelated directories or produce contradictory answers, the documentation is not yet fully polished.

---

## Core Summary

```text
Documentation mirrors the system.

Perspectives are required.
Folder templates are not.

Ownership comes before detail.

Business explains what and why.
Technical areas explain how.

Data progresses from ownership to persistence.

Technology ownership follows responsibility.
Usage is contextual.

Internal interfaces are not external integrations.

Canonical knowledge has one source of truth.
Related knowledge is connected by references.

Storage is hierarchical.
Knowledge is graph-based.

System is a synthesis layer.

Evidence beats architectural preference.

Open questions stay visible.

Human-readable documentation stays bilingual.
Machine-readable artifacts stay shared.
```
