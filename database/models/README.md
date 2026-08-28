# Data Models

> Conceptual and logical representations of Aveli data.

## Purpose

`models/` separates **system meaning from physical persistence**.

```text
Conceptual Model
    ↓
Logical Model
    ↓
Physical Persistence
```

- **Conceptual** — information concepts and relationships without storage technology.
- **Logical** — attributes, identifiers, relationships, and cardinality without a specific SQL dialect or ORM.
- **Physical** — belongs to the persistence component that owns the real storage (`../local/`, `../server/`).

## Navigation

Current model:
[`conceptual/domain-model.md`](conceptual/domain-model.md)

The logical model will be created after conceptual review.
