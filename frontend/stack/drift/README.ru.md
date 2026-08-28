# Drift

> Typed Flutter data-access technology над device-local SQLite workspace.

## Verified Version

`drift 2.34.3`

Supporting native packages: `drift_flutter`, `sqlite3_flutter_libs`.

## Role

Drift реализует schema access, repositories, reactive table updates, migrations и in-memory DB testing для per-user workspace.

## Ownership

- SQLite schema/data ownership → [`../../../database/local/`](../../../database/local/)
- Drift client usage → canonical frontend здесь

## Replaceability

**Medium.** Repository boundaries защищают screens/domain, но data implementations, generated code, migrations и DB tests связаны с Drift.
