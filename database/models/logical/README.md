# Logical Data Model

> Technology-independent structural model of Aveli data.

## Purpose

The `logical/` directory describes **which data attributes, identifiers, relationships, and cardinalities the system needs**, without committing those structures to SQLite, PostgreSQL, Drift, Prisma, or a specific SQL dialect.

The logical model is the bridge between the conceptual domain model and verified physical persistence.

## Responsibility

This area owns:

- logical entities;
- logical identifiers;
- required business attributes;
- entity relationships;
- relationship cardinality;
- optionality where it is known;
- distinctions that physical persistence must preserve.

## Boundaries

Do not define here:

- SQL data types;
- implementation table or column names;
- indexes;
- ORM annotations;
- migration syntax;
- storage paths;
- framework-specific persistence behavior.

Those details are canonical in:

- [`../../local/`](../../local/)
- [`../../server/`](../../server/)

The current logical model has already been reconciled with the verified persistence description.

## Navigation

- [`data-model.md`](data-model.md)
- [`data-model.puml`](data-model.puml)

Conceptual source:

[`../conceptual/domain-model.md`](../conceptual/domain-model.md)

Verification record:

[`../../implementation-verification.md`](../../implementation-verification.md)
