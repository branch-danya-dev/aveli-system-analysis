# Logical Data Model

> Technology-independent структурная модель данных Aveli.

## Назначение

Директория `logical/` описывает **какие attributes, identifiers, relationships и cardinalities нужны системе**, не привязывая их к SQLite, PostgreSQL, Drift, Prisma или конкретному SQL dialect.

Logical model является мостом между conceptual domain model и physical persistence.

## Ответственность

Область владеет:

- logical entities;
- logical identifiers;
- необходимыми business attributes;
- relationships между entities;
- cardinality;
- optionality там, где она уже известна;
- различиями, которые должна сохранить physical persistence.

## Границы

Здесь не определяются:

- SQL data types;
- concrete table или column names, продиктованные реализацией;
- indexes;
- ORM annotations;
- migration syntax;
- storage paths;
- framework-specific persistence behavior.

Они относятся к `../../../local/` и `../../../server/` после проверки реализации.

## Навигация

- [`data-model.ru.md`](data-model.ru.md)
- [`data-model.puml`](data-model.puml)

Conceptual source:

[`../conceptual/domain-model.ru.md`](../conceptual/domain-model.ru.md)
