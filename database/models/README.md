# Data Models

> Conceptual and logical representations of Aveli data.

## Purpose

`models/` separates **system meaning and technology-independent structure from physical persistence**.

```text
Conceptual Model
    ↓
Logical Model
    ↓
Physical Persistence
```

- **Conceptual** — information concepts and relationships without storage technology.
- **Logical** — attributes, identifiers, relationships, and cardinality without a specific SQL dialect or ORM.
- **Physical** — owned by the persistence component that stores the real data.

## Navigation

Conceptual model:

[`conceptual/domain-model.md`](conceptual/domain-model.md)

Verified logical baseline:

[`logical/data-model.md`](logical/data-model.md)

Physical persistence:

- [`../local/`](../local/)
- [`../server/`](../server/)

Persistence technologies:

[`../stack/`](../stack/)
