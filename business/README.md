# Business

> Business-level documentation for Aveli: product context, scope, requirements, processes, tasks, and traceability.

---

## Purpose

The `business/` directory is the entry point for understanding **what Aveli is, why it exists, what the system must do, and where its product boundaries are**.

It contains business and analytical documentation only. Technical implementation is described in the corresponding system areas such as `database/`, `backend/`, `frontend/`, `integrations/`, and `operations/`.

---

## Responsibility

The `business/` area is responsible for documenting:

- product context and business goals;
- system scope and product boundaries;
- functional and non-functional requirements;
- business rules and constraints;
- user and business processes;
- implementation task statements;
- acceptance criteria;
- traceability from business decisions to technical implementation.

This layer defines **intent and expected behavior**.

Technical layers define **implementation**.

---

## Boundaries

Business documentation must answer questions such as:

```text
Why does the product exist?
Who uses it?
What is in scope?
What is out of scope?
What behavior is required?
What rules must remain true?
What result is considered acceptable?
```

It should not describe implementation details unless they directly affect product behavior or scope.

Do not place details here such as:

```text
PostgreSQL
SQLite
NestJS
Flutter
JWT
Redis
Docker
specific API payloads
database schemas
deployment configuration
```

Instead, describe the business meaning and reference the canonical technical documentation.

Example:

```text
The workspace must remain available without permanent network connectivity.
```

Technical implementation:

```text
→ ../frontend/offline/
→ ../database/
```

---

## Structure

The directory is divided into focused areas for context, scope, requirements, processes, tasks, and traceability.

Each area should remain readable independently and link to deeper technical documentation when necessary.

---

## Navigation

| Area | Responsibility |
|---|---|
| `context/` | Product background, target users, goals, and positioning. |
| `scope/` | Product boundary, in-scope capabilities, exclusions, and constraints. |
| `requirements/` | Functional requirements, non-functional requirements, business rules, and acceptance criteria. |
| `processes/` | User and business workflows. |
| `tasks/` | Task statements derived from approved analytical decisions. |
| `traceability/` | Links between business rules, requirements, tasks, implementation, and verification. |

For technical implementation, continue to:

```text
../database/
../backend/
../frontend/
../integrations/
../operations/
../system/
```

---

## Documentation Rules

All documents in this directory follow the repository-wide rules defined in:

[`../rules.md`](../rules.md)

For `business/`, the main rules are:

- describe **what** and **why** before **how**;
- keep business meaning separate from technical implementation;
- explain important concepts instead of only listing them;
- reference canonical technical documentation instead of duplicating it;
- maintain both English and Russian versions;
- keep requirements and rules traceable to implementation where relevant.

---

## Reading Entry Point

A new reader should normally start with:

```text
context/
  ↓
scope/
  ↓
requirements/
  ↓
processes/
```

From there, the reader can move into the technical areas required for their role or task.
