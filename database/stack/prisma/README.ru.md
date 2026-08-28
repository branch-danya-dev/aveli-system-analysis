# Prisma

> Schema-first ORM, typed database client и migration layer NestJS backend.

## Role

Prisma связывает NestJS services с PostgreSQL:

```text
NestJS services
      ↓
    Prisma
      ↓
 PostgreSQL
```

## Current Usage

Backend использует Prisma для:

- `schema.prisma` как source определения models;
- typed database access;
- versioned SQL migrations;
- enums и relational models;
- transactions;
- direct service-level database access через `PrismaService`.

Типичный usage прямой:

```text
this.prisma.accessGrant.findMany(...)
this.prisma.$transaction(...)
```

Отдельного repository abstraction вокруг Prisma в текущем backend нет.

Raw SQL используется как исключение, а не основной access path.

## Почему подходит Aveli

Aveli имеет сравнительно небольшой relational backend, где главные engineering needs:

- schema clarity;
- typed access;
- repeatable migrations;
- low boilerplate;
- straightforward integration с NestJS services.

Prisma закрывает эти требования без большого custom data-access layer.

## Alternatives

Сопоставимые alternatives:

- TypeORM;
- Drizzle;
- Kysely + `pg`;
- raw `pg`.

В repository пока нет historical ADR, подтверждающего их formal evaluation.

## Dependencies

Prisma зависит от:

- PostgreSQL schema;
- Prisma migrations;
- NestJS services, использующих `PrismaService`;
- tests/mocks, связанных с Prisma calls.

Canonical physical data model:

[`../../server/`](../../server/)

## Replaceability

**Replaceability: medium.**

Замена Prisma при сохранении PostgreSQL и стабильных API contracts в основном затронет backend data-access layer.

Ожидаемые последствия:

- rewrite direct `this.prisma.*` service calls;
- conversion migration management;
- adaptation Prisma-specific tests и mocks;
- сохранение database constraints и API behavior.

Flutter не должен измениться, если backend API contracts останутся стабильными.

## Important Boundary

Prisma — implementation technology.

Она не является частью public API contract и не должна проникать в business documentation.
