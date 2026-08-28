# Local SQLite Schema

> Physical schema of the current per-user Aveli workspace database.

Current schema version: **11**

Runtime database rule:

```text
PRAGMA foreign_keys = ON
```

## Baseline Status

The schema is reconciled with the supplied persistence description.

One known source naming discrepancy remains for Service duration fields:

```text
duration_minutes / return_interval_minutes
vs
duration / return_interval
```

The detailed table definition (`duration`, `return_interval`) is used until direct Drift declaration verification resolves the naming difference.

## Navigation

- [`schema.md`](schema.md) — tables, relationships, denormalization, indexes.
- [`enums.md`](enums.md) — persisted local enum values.
- [`schema.puml`](schema.puml) — physical ER diagram.
