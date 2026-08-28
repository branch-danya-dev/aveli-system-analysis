# Prisma

> Schema-first ORM, typed database client, and migration layer for the NestJS backend.

## Role

Prisma connects NestJS services with PostgreSQL:

```text
NestJS services
      ↓
    Prisma
      ↓
 PostgreSQL
```

## Current Usage

The backend uses Prisma for:

- `schema.prisma` as the model definition source;
- typed database access;
- versioned SQL migrations;
- enums and relational models;
- transactions;
- direct service-level database access through `PrismaService`.

Typical service usage is direct:

```text
this.prisma.accessGrant.findMany(...)
this.prisma.$transaction(...)
```

There is no separate repository abstraction around Prisma in the current backend.

Raw SQL is exceptional rather than the normal access path.

## Why It Fits Aveli

Aveli has a relatively small relational backend where the main engineering needs are:

- schema clarity;
- typed access;
- repeatable migrations;
- low boilerplate;
- straightforward use from NestJS services.

Prisma satisfies those needs without requiring a large custom data-access layer.

## Alternatives

Comparable alternatives include:

- TypeORM;
- Drizzle;
- Kysely + `pg`;
- raw `pg`.

The repository does not currently contain a historical ADR proving that these alternatives were formally evaluated.

## Dependencies

Prisma depends on:

- PostgreSQL schema;
- Prisma migrations;
- NestJS services using `PrismaService`;
- tests/mocks coupled to Prisma calls.

Canonical physical data model:

[`../../server/`](../../server/)

## Replaceability

**Replaceability: medium.**

Replacing Prisma while keeping PostgreSQL and stable API contracts would mainly affect the backend data-access layer.

Expected impact:

- rewrite direct `this.prisma.*` service calls;
- convert migration management;
- adapt Prisma-specific tests and mocks;
- preserve database constraints and API behavior.

Flutter should remain unaffected if backend API contracts remain stable.

## Important Boundary

Prisma is an implementation technology.

It is not part of the public API contract and should not leak into business documentation.
