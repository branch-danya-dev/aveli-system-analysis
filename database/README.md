# Database

> Canonical documentation for Aveli data ownership, logical models, physical persistence, and storage-engine technologies.

## Status

**Baseline: Stable**

The database baseline is reconciled with verified local/server persistence evidence. One non-blocking source naming discrepancy for Service duration fields remains explicitly documented.

## Purpose

`database/` answers:

```text
What data exists?
Who owns it?
How is it logically related?
Where is it physically persisted?
How does persistence evolve?
```

## Structure

```text
database/
├── architecture/
├── models/
│   ├── conceptual/
│   └── logical/
├── stack/
├── local/
├── server/
└── diagrams/
```

| Area | Responsibility |
|---|---|
| `architecture/` | Ownership, source-of-truth, isolation, lifecycle. |
| `models/` | Conceptual/logical data models. |
| `stack/` | Storage engines: SQLite and PostgreSQL. |
| `local/` | Device-side physical persistence and files. |
| `server/` | PostgreSQL physical persistence. |
| `diagrams/` | Database knowledge maps. |

## Technology Boundary

Canonical storage technologies:

```text
SQLite      → database/stack/sqlite/
PostgreSQL  → database/stack/postgresql/
```

Data-access technologies belong to their runtime components:

```text
Drift   → ../frontend/stack/drift/
Prisma  → ../backend/stack/prisma/
```

## Reading Path

```text
Data ownership
   ↓
Conceptual model
   ↓
Logical model
   ↓
Storage engines
   ↓
Local / server physical models
```

Start with [`architecture/data-ownership.md`](architecture/data-ownership.md).

Verification record: [`implementation-verification.md`](implementation-verification.md)

## Core Principle

> Data documentation progresses from ownership to conceptual structure, then logical structure, and only then to physical persistence.

## Documentation Rules

[`../rules.md`](../rules.md)
