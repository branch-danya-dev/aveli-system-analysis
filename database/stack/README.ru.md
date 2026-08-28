# Database Stack

> Каноническая документация persistence technologies Aveli.

## Назначение

Директория `stack/` объясняет **какие persistence technologies используются, почему они подходят системе, где применяются, что от них зависит и к чему приведет их замена**.

Canonical technology documentation находится здесь.

Concrete schemas, entities, constraints и migrations остаются у:

```text
../local/
../server/
```

## Текущий Stack

| Technology | Role |
|---|---|
| `SQLite` | Local relational storage engine профессионального workspace. |
| `Drift` | Typed Flutter persistence/data-access layer поверх SQLite. |
| `PostgreSQL` | Backend relational database для identity, access, billing state и webhook events. |
| `Prisma` | Schema-first ORM, typed client и migration layer NestJS backend. |

## Навигация

- [`sqlite/`](sqlite/)
- [`drift/`](drift/)
- [`postgresql/`](postgresql/)
- [`prisma/`](prisma/)

## Правило Selection Rationale

Текущая implementation дает сильное техническое обоснование этих решений.

При этом в репозитории нет historical ADR, подтверждающего, что каждая указанная альтернатива была формально рассмотрена до реализации.

Поэтому stack documentation различает:

```text
Current rationale        → подтверждается структурой и implementation
Historical decision log  → формально не зафиксирован
```

Rejected alternatives нельзя описывать как исторический факт без ADR или эквивалентного evidence.
