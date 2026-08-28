# Data Architecture

> Ownership, source-of-truth boundaries, isolation и lifecycle данных Aveli.

## Назначение

`architecture/` определяет **где начинаются и заканчиваются ответственности за данные до рассмотрения физических схем**.

## Ответственность

Область владеет:
- границами data domains;
- canonical data authority;
- user-workspace isolation;
- сохранностью данных при logout и изменении access;
- lifecycle rules, общими для нескольких persistence components.

## Границы

Здесь не определяются table columns, SQL types, ORM models, indexes или migration syntax.

## Навигация

- [`data-ownership.ru.md`](data-ownership.ru.md)
- [`data-lifecycle.ru.md`](data-lifecycle.ru.md)

Канонические business rules:
[`../../business/requirements/business-rules.ru.md`](../../business/requirements/business-rules.ru.md)
