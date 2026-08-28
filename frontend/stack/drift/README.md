# Drift

> Typed Flutter data-access technology over the device-local SQLite workspace.

## Verified Version

`drift 2.34.3`

Supporting native packages include `drift_flutter` and `sqlite3_flutter_libs`.

## Role

Drift implements schema access, repositories, reactive table updates, migrations and in-memory database testing for the per-user workspace.

## Ownership

- SQLite schema/data ownership → [`../../../database/local/`](../../../database/local/)
- Drift client usage → frontend canonical here

## Replaceability

**Medium.** Repository boundaries protect screens/domain, but data implementations, generated code, migration logic and database tests are coupled to Drift.
