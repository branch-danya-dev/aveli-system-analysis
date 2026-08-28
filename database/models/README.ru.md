# Data Models

> Conceptual и logical представления данных Aveli.

## Назначение

`models/` отделяет **смысл системы и technology-independent структуру от physical persistence**.

```text
Conceptual Model
    ↓
Logical Model
    ↓
Physical Persistence
```

- **Conceptual** — information concepts и relationships без storage technology.
- **Logical** — attributes, identifiers, relationships и cardinality без конкретного SQL dialect или ORM.
- **Physical** — принадлежит persistence component, который хранит реальные данные.

## Навигация

Conceptual model:

[`conceptual/domain-model.ru.md`](conceptual/domain-model.ru.md)

Verified logical baseline:

[`logical/data-model.ru.md`](logical/data-model.ru.md)

Physical persistence:

- [`../local/`](../local/)
- [`../server/`](../server/)

Persistence technologies:

[`../stack/`](../stack/)
