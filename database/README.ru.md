# Database

> Каноническая документация Aveli по ownership данных, моделям, persistence boundaries, physical schemas и persistence technologies.

## Назначение

`database/` объясняет **какие данные существуют, кто ими владеет, как они моделируются, где находится canonical state и как данные физически хранятся**.

## Структура

```text
database/
├── architecture/
├── models/
│   ├── conceptual/
│   └── logical/
├── local/
├── server/
├── stack/
└── diagrams/
```

| Область | Ответственность |
|---|---|
| `architecture/` | Ownership, source-of-truth boundaries, isolation и lifecycle. |
| `models/` | Conceptual и logical data models. |
| `local/` | Проверенный device-side persistence. |
| `server/` | Проверенный backend persistence. |
| `stack/` | Canonical persistence-technology documentation. |
| `diagrams/` | Cross-area database knowledge maps. |

## Путь чтения

```text
Architecture
    ↓
Conceptual Model
    ↓
Logical Model
    ↓
Local / Server Physical Persistence
    ↓
Persistence Stack
```

Начать с:

[`architecture/data-ownership.ru.md`](architecture/data-ownership.ru.md)

Визуальная карта:

[`diagrams/database-map.puml`](diagrams/database-map.puml)

## Основной принцип

> **Data documentation progresses from ownership to conceptual structure, from conceptual structure to logical structure, and only then to physical persistence.**

Physical models остаются у persistence component, который хранит данные.

## Правила документации

[`../rules.ru.md`](../rules.ru.md)
