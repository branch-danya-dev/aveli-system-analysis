# Local Persistence

> Verified persistence model for the Aveli professional workspace on the user's device.

## Purpose

`local/` documents the physical persistence owned by the device-side workspace.

The current verified model uses:

```text
Per-user SQLite database
+
Visit-photo files
+
Secure client-side access/session state
```

## Core Boundary

Each authenticated user opens a dedicated SQLite database:

```text
documents/aveli_<userId>.sqlite
```

The server user UUID is used to select the local database and photo path, but local business entities do **not** contain foreign keys to the server `users` table.

Professional workspace data is not synchronized to the backend.

## Navigation

| Area | Responsibility |
|---|---|
| `schema/` | Current SQLite schema and physical relationships. |
| `entities/` | Table-level physical documentation. |
| `migrations/` | Drift schema history. |
| `files/` | Visit-photo file persistence. |
| `secure-storage/` | Persistence boundary for access snapshot and auth tokens outside SQLite. |

## Verified Implementation Sources

- `lib/core/database/app_database.dart`
- `lib/core/database/tables/*.dart`
- `lib/core/database/local_database_manager.dart`
