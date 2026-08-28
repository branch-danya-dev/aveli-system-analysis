# SQLite

> Local relational storage engine for the Aveli professional workspace.

## Role

SQLite is the physical database engine behind each user's local professional workspace.

```text
User
  ↓
aveli_<userId>.sqlite
  ↓
clients / services / appointments / payments / notes / photo metadata / settings
```

The database remains device-local and is not synchronized to the Aveli backend.

## Why It Fits Aveli

Aveli's workspace is relational rather than document-oriented.

The current schema relies on:

- foreign keys between appointments, clients, and services;
- `UNIQUE(appointment_id)` for one payment row per appointment;
- cascade behavior for visit-related records;
- joins used by Today, Calendar, client history, and outstanding payments;
- a long-lived versioned schema with 11 migration versions;
- a portable per-user `.sqlite` file.

The important architectural choice is therefore not simply “SQLite instead of another Flutter package”.

It is:

> **relational persistence instead of document/key-value persistence for the professional workspace.**

## Why Not Hive / Isar for the Current Model

Hive is well suited to lightweight key-value state and caches, but the Aveli workspace depends on relational constraints and joins.

Isar can model connected data, but replacing SQLite would also require replacing existing relational constraints, migrations, reactive data access, and the user database format.

This would be a storage-model migration rather than a simple library replacement.

## Dependencies

Canonical physical usage:

- [`../../local/schema/`](../../local/schema/)
- [`../../local/entities/`](../../local/entities/)
- [`../../local/migrations/`](../../local/migrations/)

Consumer technology:

- [`../drift/`](../drift/)

## Replaceability

**Replaceability: medium at the engine/file-format level, but low-priority to replace for the current product.**

Replacing SQLite with another storage model would affect:

- user-data migration;
- relationships and constraints;
- Calendar / Today query projections;
- payment invariants;
- import/export;
- persistence tests.

Replacing Drift while keeping SQLite is significantly cheaper than replacing SQLite itself.

## Alternatives

Relevant alternatives include:

- Isar;
- Hive;
- another SQLite access layer such as `sqflite`.

The repository does not currently contain a formal historical ADR proving that these were evaluated before implementation.
