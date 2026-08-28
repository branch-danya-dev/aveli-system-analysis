# Local SQLite Schema

> Physical schema текущей per-user workspace database Aveli.

Текущая schema version: **11**

Runtime database rule:

```text
PRAGMA foreign_keys = ON
```

## Baseline Status

Schema сверена с предоставленным persistence description.

Остается одно известное source naming discrepancy для Service duration fields:

```text
duration_minutes / return_interval_minutes
vs
duration / return_interval
```

До прямой проверки Drift declarations используется detailed table definition: `duration`, `return_interval`.

## Навигация

- [`schema.ru.md`](schema.ru.md) — tables, relationships, denormalization, indexes.
- [`enums.ru.md`](enums.ru.md) — persisted local enum values.
- [`schema.puml`](schema.puml) — physical ER diagram.
