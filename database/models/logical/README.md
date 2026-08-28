# Logical Data Model

> Technology-independent structural model of Aveli data.

## Purpose

The `logical/` directory describes **which data attributes, identifiers, relationships, and cardinalities the system needs**, without committing those structures to SQLite, PostgreSQL, Drift, Prisma, or a specific SQL dialect.

The logical model is the bridge between the conceptual domain model and physical persistence.

## Responsibility

This area owns:

- logical entities;
- logical identifiers;
- required business attributes;
- entity relationships;
- relationship cardinality;
- optionality where it is already known;
- distinctions that physical persistence must preserve.

## Boundaries

Do not define here:

- SQL data types;
- concrete table or column names required by implementation;
- indexes;
- ORM annotations;
- migration syntax;
- storage paths;
- framework-specific persistence behavior.

Those belong to `../../../local/` and `../../../server/` after implementation verification.

## Navigation

- [`data-model.md`](data-model.md)
- [`data-model.puml`](data-model.puml)

Conceptual source:

[`../conceptual/domain-model.md`](../conceptual/domain-model.md)
