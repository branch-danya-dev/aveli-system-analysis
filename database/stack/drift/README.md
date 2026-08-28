# Drift

> Typed Flutter persistence and data-access layer over SQLite.

## Role

Drift is not the storage engine.

It provides the Flutter-side abstraction over SQLite:

```text
Flutter repositories
        ↓
      Drift
        ↓
      SQLite
```

## Current Usage

The implementation uses Drift for:

- schema definition;
- schema versioning and migrations;
- typed queries and generated companions;
- generated database code;
- reactive `.watch()` / `tableUpdates`;
- in-memory database testing.

Reactive table updates are used to refresh Calendar / Today behavior without a separate manual invalidation system.

## Why It Fits Aveli

Drift preserves direct access to a relational SQLite model while adding:

- compile-time typing;
- generated query/model code;
- migration support;
- reactive streams;
- testable in-memory database behavior.

This fits a Flutter application whose data layer performs relational reads and must react to local changes.

## Closest Alternative

The nearest substitute is not Isar or Hive.

It is another access layer over SQLite, especially:

```text
sqflite + manual SQL
```

That would preserve the `.sqlite` storage model but require more hand-written data-access and reactive plumbing.

## Dependencies

Drift depends on:

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

Replacing Drift while preserving SQLite would require:

- rewriting repository implementations;
- replacing `Companion`, typed selects, and generated models;
- replacing `.watch()` / `tableUpdates` behavior;
- adapting database tests;
- preserving or translating migrations.

User screens and higher-level domain use cases should not require a full rewrite if repository/domain boundaries remain stable.

## Alternative Classes

- `sqflite` + manual SQL;
- Floor;
- another SQLite-oriented Flutter data layer.

Moving to Isar/Hive is not an ORM replacement; it is a persistence-model change.
