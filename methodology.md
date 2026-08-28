# System-Structured Analysis Documentation

> **Working name:** System-Structured Analysis Documentation (SSAD)  
> **Status:** Evolving methodology, validated on the Aveli case  
> **Purpose:** Explain the reasoning model behind this repository structure.

[Русская версия](methodology.ru.md) · [Repository rules](rules.md)

---

## 1. What SSAD Is

SSAD organizes system-analysis documentation around the **actual responsibilities and boundaries of a system**, rather than around a fixed list of analyst artifact types.

The core idea is:

> **Documentation should mirror the system being analyzed.**

A repository should therefore read less like:

```text
requirements/
diagrams/
api/
security/
```

and more like the system itself:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

when those areas really exist.

The physical tree is not the methodology. The methodology is the ownership model behind the tree.

A plugin, a mobile product, and a distributed banking platform should not be forced into identical directory structures.

---

## 2. Methodology vs Rules

This file explains why the approach exists, how knowledge is constructed, how ownership is chosen, and how system perspectives relate.

[`rules.md`](rules.md) defines the normative repository contract: what must be documented, what must not be duplicated, where knowledge belongs, and how consistency is reviewed.

The methodology may evolve from project experience. The rules should change only when that evolution becomes an explicit repository standard.

---

## 3. The Repository Is a System Model

Documentation should support progressive depth:

```text
System
  ↓
Responsibility / Component
  ↓
Behavior / Contract / Data
  ↓
Technology
  ↓
Usage
  ↓
Implementation evidence
```

Different readers can enter at different points. A product reader may start from `business/`; a system analyst may move between `business/`, `system/`, `database/`, `backend/`, `frontend/`, and `integrations/`; a developer can enter directly through the component they own.

The reader should not be forced through one linear sequence to understand one local concern.

---

## 4. Storage Is Hierarchical; Knowledge Is Graph-Based

Files and folders are hierarchical because ownership and navigation need a tree. Real system knowledge is not hierarchical.

Authentication can simultaneously involve:

```text
business requirements
backend session logic
server data
frontend bootstrap
secure storage
access policy
security constraints
```

The repository therefore has two simultaneous structures:

```text
Physical hierarchy
→ where canonical knowledge lives

Cross-reference graph
→ how the system actually fits together
```

> **Storage is hierarchical. Knowledge is graph-based.**

---

## 5. Canonical Knowledge and Contextual Usage

Every important fact should have one canonical owner.

Examples:

```text
backend/api/
→ concrete backend HTTP contracts

database/
→ canonical data architecture and physical persistence

frontend/stack/drift/
→ Drift as a frontend data-access technology

integrations/revenuecat/
→ the cross-component RevenueCat boundary
```

Other documents may repeat enough context to remain readable, but they should link back to the owner instead of creating a second source of truth.

> **Do not duplicate knowledge. Duplicate context when necessary.**

---

## 6. Perspectives Are Required; Folder Templates Are Not

A project must answer certain analytical questions even when its physical directory tree differs.

At minimum, the analysis should make it possible to understand:

```text
Product / business intent
System boundary and scope
Data ownership and lifecycle
Runtime responsibilities / components
Interfaces and external integrations
Technology choices and usages
Failure / trust / security constraints
Verification / acceptance
Whole-system relationships
```

For Aveli, those questions naturally became:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

For another system, `backend/` or `frontend/` may be inapplicable, while `worker/`, `plugin/`, `gateway/`, or `operations/` may deserve top-level ownership.

> **Make every important system perspective explicit, and let the real architecture determine its physical owner.**

---

## 7. Business and Technical Knowledge Stay Connected but Separate

Business documentation answers:

```text
Why does this exist?
Who needs it?
What is in scope?
What behavior must remain true?
What result is acceptable?
```

Technical documentation answers:

```text
How is that behavior realized?
Which component owns it?
Which data and interfaces participate?
Which technology supports it?
How can it fail?
```

A technical fact belongs in business documentation only when it materially changes product behavior, scope, an external contract, or a user-visible capability.

Prefer product truth such as:

```text
Account state is server-managed.
Professional workspace data is not synchronized between devices.
```

instead of implementation detail such as:

```text
Stored in PostgreSQL.
Implemented with NestJS.
```

---

## 8. Ownership Before Detail

Before documenting implementation, determine who owns the responsibility.

For important behavior, ask:

```text
Who decides this?
Who stores the canonical state?
Who may change it?
Who only consumes it?
Who verifies it?
```

Only after ownership is understood should the analysis finalize APIs, schemas, state models, technologies, or UI behavior.

---

## 9. Data Documentation Progresses from Ownership to Persistence

Data should be modeled in dependency order:

```text
Data ownership
    ↓
Conceptual model
    ↓
Logical model
    ↓
Physical persistence model
    ↓
Migrations / constraints / lifecycle
```

A system-wide logical model should not erase real storage boundaries. Physical models belong to the persistence owner that stores them.

When one product uses local SQLite and server PostgreSQL, those physical models remain distinct even if the logical domain view connects them.

---

## 10. Technology Is an Analytical Object

Technology choices should answer more than "we use X".

A meaningful technology document should explain, where known:

```text
What responsibility does it support?
Why is it used here?
Where is it used?
What depends on it?
What limitations does it introduce?
How critical is it?
Could it be replaced?
What alternatives matter?
```

Technology ownership follows the responsibility it primarily realizes.

Examples from Aveli:

```text
SQLite / PostgreSQL
→ database persistence technologies

Drift
→ frontend data-access technology

Prisma
→ backend data-access technology

RevenueCat
→ external integration with frontend and backend consumers
```

Contextual usage can appear elsewhere, but canonical technology knowledge should not be duplicated across perspectives.

---

## 11. Three Dimensions of Technical Knowledge

SSAD separates three connected questions.

### Structural knowledge

What exists?

### Technology knowledge

What is used?

### Usage knowledge

Where and why does each technology participate?

Example:

```text
Drift
→ local repository implementations
→ reactive workspace data
→ local migrations
```

Keeping these dimensions distinct improves both human navigation and automated analysis.

---

## 12. Internal Interfaces and External Integrations Are Different Boundaries

An internal interface connects components inside the analyzed system:

```text
Aveli Mobile
↕
Aveli Backend
```

Its concrete contracts belong to the component that owns them, such as `backend/api/`.

An external integration crosses the system boundary:

```text
Aveli
↕
RevenueCat / App Store / Google Play / OS service / third-party API
```

Integration documentation owns the cross-boundary relationship: external responsibility, identity mapping, exchanged data, trust, authentication, failure behavior, and reconciliation.

This prevents `integrations/` from becoming a generic bucket for every API call.

---

## 13. System View Is a Synthesis Layer

The final `system/` view should normally be built after major component perspectives are understood.

It owns knowledge that cannot be assigned to only one component:

```text
system context
component relationships
end-to-end flows
cross-component data movement
trust / authority map
system invariants
boundary-changing evolution
whole-system failure scenarios
```

It should not become another copy of backend, frontend, database, or integration documentation.

> **A layer may reference another layer, but a model that describes several layers together belongs to their nearest common system-level owner.**

A preliminary system sketch may exist early. The final system model should be synthesized from stabilized lower-level evidence.

---

## 14. Dependency-Driven Construction

Documentation has dependencies just like implementation.

A useful default direction is:

```text
Existing evidence / product reality
        ↓
Business context and scope
        ↓
Rules / requirements / processes
        ↓
Initial responsibility boundaries
        ↓
Data ownership and domain model
        ↓
Interfaces and external constraints
        ↓
Component architecture
        ↓
Technology and implementation evidence
        ↓
System-wide synthesis
        ↓
Traceability / open questions / final consistency review
```

The exact order can change when an external constraint is upstream.

> **Build knowledge in dependency order and stabilize upstream decisions before detailing downstream implementation.**

---

## 15. Evidence-First Documentation

When documenting an existing implementation, begin with evidence:

```text
source code
schemas
API contracts
configuration
native project files
provider contracts
current behavior
tests
existing documentation
stakeholder decisions
```

Separate what is:

```text
VERIFIED
INFERRED
OPEN
```

Do not convert an assumption into system truth because it makes the architecture look cleaner. A target-state proposal should be explicitly identified as target state.

---

## 16. Stabilization and Review

A useful conceptual lifecycle is:

```text
Draft
  ↓
Baseline
  ↓
Stable
```

- **Draft** — the system is still being discovered.
- **Baseline** — stable enough to become upstream input for deeper work.
- **Stable** — cross-checked against relevant implementation, contracts, or stakeholder decisions and suitable as canonical knowledge.

A downstream discovery may still reopen an upstream decision. The goal is not to avoid iteration; it is to avoid accidental rework caused by detailing downstream implementation before upstream ownership is understood.

---

## 17. Tasks, Changes, and Optional Perspectives

A task is not inherently a business artifact. It belongs to the area of change it describes:

```text
backend
frontend
database
integration
operations
cross-system
```

Task/change directories should appear only when real artifacts justify them.

The same applies to optional perspectives such as `operations/`. Create one when the system has enough independent operational responsibility — deployment, observability, backups, incident recovery, release operations — to justify an owner.

---

## 18. Traceability Is Navigation Between Abstraction Levels

Traceability should allow important decisions to be followed both forward and backward:

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

Not every trace requires every step. The important questions are:

```text
Why does this component or technology exist?
How does this business rule survive implementation?
How do we know the behavior is verified?
```

---

## 19. Human-Readable and Machine-Analyzable Knowledge

The primary repository is written for people, but its structure should also make automated review possible.

Future metadata may describe relationships such as:

```yaml
technology: drift
owner: frontend
used_by:
  - local repositories
supports:
  - local-first workspace
criticality: high
replaceability: medium
```

This can support cross-project questions about repeated technologies, missing traceability, dependency risk, and architecture patterns.

AI can help discover contradictions, stale links, uncovered requirements, and dependency gaps. It does not replace responsibility ownership or human-readable explanation.

---

## 20. Bilingual Documentation

Human-readable project documentation may be maintained in paired languages:

```text
README.md
README.ru.md
```

Both versions represent the same knowledge, not independent documents.

Machine-readable artifacts remain shared where translation creates meaningless duplication:

```text
openapi.yaml
schema.sql
PlantUML source
JSON / YAML schemas
```

Implementation identifiers are not translated.

---

## 21. What SSAD Is Not

SSAD does not replace UML, BPMN, C4, ADRs, OpenAPI, SQL/schema documentation, source code, tests, product management, or operational runbooks.

It is a way to **organize and connect those forms of knowledge around the system itself**.

It also does not claim that its individual practices are novel. It combines established documentation practices around a system-oriented ownership model and tests that model against real projects.

---

## 22. Aveli as the First Full Validation Case

Aveli exposed several useful methodology corrections:

- mandatory analytical questions are more important than mandatory folder names;
- data ownership should be stabilized before physical schemas and many APIs;
- technology ownership should follow responsibility, not whichever document first mentions a library;
- an internal API is not automatically an external integration;
- `system/` works best as a late synthesis layer;
- failure scenarios and release readiness can become system-level review views without forcing a permanent `operations/` branch;
- legacy artifact-oriented documentation should be removed only after an orphan-knowledge check.

The resulting Aveli structure is:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

with supporting screenshots, rendered diagrams, methodology, and repository rules.

This structure is a result of Aveli's architecture, not a template every project must copy.

---

## Working Definition

> **System-Structured Analysis Documentation is an evolving documentation methodology in which analytical and technical knowledge is organized around the actual responsibilities and boundaries of a system, while canonical ownership, contextual references, traceability, and system-level synthesis connect business intent to implementation and verification.**

In compact form:

```text
system-shaped hierarchy
+
canonical ownership
+
progressive depth
+
contextual usage
+
evidence-first construction
+
explicit technology modeling
+
traceability
+
system synthesis
+
human-readable knowledge graph
```

The methodology should continue to evolve from real project evidence rather than from theoretical completeness.
