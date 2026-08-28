# Data Models

> Conceptual и logical представления данных Aveli.

## Назначение

`models/` отделяет **смысл системы от physical persistence**.

```text
Conceptual Model
    ↓
Logical Model
    ↓
Physical Persistence
```

- **Conceptual** — information concepts и relationships без storage technology.
- **Logical** — attributes, identifiers, relationships и cardinality без конкретного SQL dialect или ORM.
- **Physical** — принадлежит persistence component, владеющему реальным storage (`../local/`, `../server/`).

## Навигация

Текущая модель:
[`conceptual/domain-model.ru.md`](conceptual/domain-model.ru.md)

Logical model будет создана после review conceptual model.
