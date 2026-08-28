# Prisma

> Schema-first ORM, typed database client и migration layer NestJS backend.

## Role

```text
NestJS services
      ↓
Prisma
      ↓
PostgreSQL
```

Current usage: `schema.prisma`, typed DB access, migrations, enums/relations, transactions, direct `PrismaService`. Separate repository abstraction отсутствует.

Canonical physical model: [`../../../database/server/`](../../../database/server/)

## Replaceability

**Medium.** Replacement затронет backend data access, migration tooling и tests при сохранении PostgreSQL/API contracts.

Prisma — backend implementation technology, не public API/business concept.
