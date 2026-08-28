# Prisma

> Schema-first ORM, typed database client, and migration layer for the NestJS backend.

## Role

```text
NestJS services
      ↓
Prisma
      ↓
PostgreSQL
```

Current usage includes `schema.prisma`, typed database access, versioned migrations, enums/relationships, transactions, and direct `PrismaService` use. There is no separate repository abstraction around Prisma in the current backend.

## Why It Fits

Aveli backend is a compact relational service where schema clarity, typed access, repeatable migrations and low data-access boilerplate are useful.

## Dependencies

- PostgreSQL physical model;
- Prisma migrations;
- backend services using `PrismaService`;
- Prisma-coupled tests/mocks.

Canonical physical model: [`../../../database/server/`](../../../database/server/)

## Replaceability

**Medium.** Replacing Prisma while preserving PostgreSQL/API contracts mainly affects backend data access, migration tooling and related tests.

## Alternatives

TypeORM, Drizzle, Kysely + `pg`, raw `pg`. No historical ADR proves a formal pre-implementation comparison.

## Boundary

Prisma is backend implementation technology, not a public API or business concept.
