# PostgreSQL

> Backend relational database для identity, access, subscription state и event persistence Aveli.

## Role

PostgreSQL хранит backend-owned domain:

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

Professional workspace здесь не хранится.

## Почему подходит Aveli

Backend dataset сравнительно небольшой, но product-critical.

Его ценность — в relational consistency и constraints, а не в horizontal scale.

Current schema использует возможности PostgreSQL:

- CHECK constraints;
- partial UNIQUE indexes;
- relational foreign keys;
- transactions;
- JSONB для raw subscription-event payloads;
- concurrent backend/webhook writes.

Критичный пример — partial UNIQUE index, обеспечивающий один registration trial на user.

## Почему не MongoDB для текущей модели

Backend data strongly related и constraint-heavy.

Для текущего небольшого набора relational tables MongoDB перенес бы важные invariants из database в application logic без заметной product value.

## Другие SQL Engines

MySQL или server-side SQLite способны хранить большую часть этих данных, но migration потребует отдельной проверки:

- partial-index behavior;
- CHECK semantics;
- JSONB usage;
- concurrency characteristics;
- generated migrations.

## Dependencies

Canonical physical usage:

- [`../../server/schema/`](../../server/schema/)
- [`../../server/entities/`](../../server/entities/)
- [`../../server/constraints/`](../../server/constraints/)
- [`../../server/migrations/`](../../server/migrations/)

Consumer technology:

- [`../prisma/`](../prisma/)

## Replaceability

**Replaceability: medium.**

Backend API boundary изолирует Flutter от database engine, но замена PostgreSQL всё равно потребует:

- conversion migrations;
- verification constraints;
- изменения JSON representation, где применимо;
- operational/deployment changes;
- regression testing access и billing invariants.

## Selection Status

Current technical rationale сильный.

Historical formal comparison всех альтернатив в repository не зафиксирован.
