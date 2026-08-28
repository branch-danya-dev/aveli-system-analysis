# Drift

> Typed Flutter data-access technology over the device-local SQLite workspace.

## Verified Version

`drift 2.34.3`

## Role

```text
Flutter repositories
      ↓
Drift
      ↓
SQLite
```

Current usage includes schema/table declarations, typed queries/generated companions, schema versioning/migrations, reactive `.watch()` / table updates, generated database code, and in-memory database testing.

Canonical physical model: [`../../../database/local/`](../../../database/local/)

## Replaceability

**Medium.** Replacing Drift while retaining SQLite affects repository implementations, generated-model/Companion usage, reactive reads, migration tooling and database tests.

Closest alternatives are SQLite-oriented access layers such as `sqflite` + manual SQL or Floor. Moving to a non-relational store would be a persistence-model change, not only an ORM replacement.
