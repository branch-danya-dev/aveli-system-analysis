# Database

> Canonical documentation Aveli по data ownership, logical models, physical persistence и storage-engine technologies.

## Статус

**Baseline: Stable**

Database baseline reconciled с verified local/server persistence evidence. Один non-blocking source naming discrepancy Service duration fields остаётся явно documented.

## Назначение

`database/` отвечает:

```text
Какие data существуют?
Кто ими владеет?
Как они logically связаны?
Где physical persistence?
Как persistence evolves?
```

## Структура

```text
database/
├── architecture/
├── models/
│   ├── conceptual/
│   └── logical/
├── stack/
├── local/
├── server/
└── diagrams/
```

| Area | Responsibility |
|---|---|
| `architecture/` | Ownership, source-of-truth, isolation, lifecycle. |
| `models/` | Conceptual/logical data models. |
| `stack/` | Storage engines: SQLite и PostgreSQL. |
| `local/` | Device-side physical persistence/files. |
| `server/` | PostgreSQL physical persistence. |
| `diagrams/` | Database knowledge maps. |

## Technology Boundary

```text
SQLite      → database/stack/sqlite/
PostgreSQL  → database/stack/postgresql/
Drift       → ../frontend/stack/drift/
Prisma      → ../backend/stack/prisma/
```

## Путь чтения

```text
Data ownership
   ↓
Conceptual model
   ↓
Logical model
   ↓
Storage engines
   ↓
Local / server physical models
```

Начать: [`architecture/data-ownership.ru.md`](architecture/data-ownership.ru.md)

Verification: [`implementation-verification.ru.md`](implementation-verification.ru.md)

## Documentation Rules

[`../rules.ru.md`](../rules.ru.md)
