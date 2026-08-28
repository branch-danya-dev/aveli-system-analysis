# Database

> Каноническая документация Aveli по ownership данных, моделям, persistence boundaries, physical schemas и persistence technologies.

## Назначение

`database/` объясняет **какие данные существуют, кто ими владеет, как они моделируются, какие persistence technologies используются, где находится canonical state и как данные физически хранятся**.

## Статус

**Baseline: Stable**

Текущая database documentation сверена с предоставленным описанием реальной persistence implementation.

Остается одно известное расхождение имен полей Service duration в исходных материалах. Оно явно зафиксировано в local schema и не меняет документированный смысл данных.

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

| Область | Ответственность |
|---|---|
| `architecture/` | Ownership, source-of-truth boundaries, isolation и lifecycle. |
| `models/` | Conceptual и logical data models. |
| `stack/` | Canonical persistence-technology decisions и replaceability. |
| `local/` | Проверенный device-side persistence usage. |
| `server/` | Проверенный backend persistence usage. |
| `diagrams/` | Cross-area database knowledge maps. |

## Путь чтения

```text
Architecture
    ↓
Conceptual Model
    ↓
Logical Model
    ↓
Persistence Stack
    ↓
Local / Server Physical Usage
```

Порядок соответствует правилу репозитория: canonical stack knowledge документируется до contextual technology usage.

Начать с:

[`architecture/data-ownership.ru.md`](architecture/data-ownership.ru.md)

Verification record:

[`implementation-verification.ru.md`](implementation-verification.ru.md)

Визуальная карта:

[`diagrams/database-map.puml`](diagrams/database-map.puml)

## Основной принцип

> **Data documentation progresses from ownership to conceptual structure, from conceptual structure to logical structure, and only then to technology selection and physical persistence usage.**

Physical models остаются у persistence component, который хранит данные.

## Правила документации

[`../rules.ru.md`](../rules.ru.md)
