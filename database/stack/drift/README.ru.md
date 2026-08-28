# Drift

> Typed Flutter persistence и data-access layer поверх SQLite.

## Role

Drift не является storage engine.

Он дает Flutter-side abstraction над SQLite:

```text
Flutter repositories
        ↓
      Drift
        ↓
      SQLite
```

## Current Usage

Implementation использует Drift для:

- schema definition;
- schema versioning и migrations;
- typed queries и generated companions;
- generated database code;
- reactive `.watch()` / `tableUpdates`;
- in-memory database tests.

Reactive table updates используются для обновления Calendar / Today без отдельной manual invalidation system.

## Почему подходит Aveli

Drift сохраняет relational SQLite model и добавляет:

- compile-time typing;
- generated query/model code;
- migration support;
- reactive streams;
- testable in-memory database behavior.

Это подходит Flutter application, data layer которой выполняет relational reads и должен реагировать на локальные изменения.

## Ближайшая альтернатива

Ближайшая замена — не Isar и не Hive.

Это другой access layer поверх SQLite, прежде всего:

```text
sqflite + manual SQL
```

Так можно сохранить `.sqlite` storage model, но придется писать больше data-access и reactive plumbing вручную.

## Dependencies

Drift зависит от:

- SQLite;
- current table definitions;
- migration history;
- generated code;
- repository implementations;
- database-focused tests.

Canonical physical model:

[`../../local/`](../../local/)

## Replaceability

**Replaceability: medium.**

Замена Drift при сохранении SQLite потребует:

- переписать repository implementations;
- заменить `Companion`, typed selects и generated models;
- заменить `.watch()` / `tableUpdates`;
- адаптировать database tests;
- сохранить или преобразовать migrations.

User screens и higher-level domain use cases не должны требовать полного rewrite, если repository/domain boundaries сохранятся.

## Alternative Classes

- `sqflite` + manual SQL;
- Floor;
- другой SQLite-oriented Flutter data layer.

Переход на Isar/Hive — это не ORM replacement, а смена persistence model.
