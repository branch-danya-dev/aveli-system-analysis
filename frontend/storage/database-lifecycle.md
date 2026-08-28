# Local Database Lifecycle

## Canonical Physical Schema

See:

[`../../database/local/`](../../database/local/)

This document owns Flutter-side lifecycle usage.

## Verified Behavior

| Concern | Frontend behavior |
|---|---|
| Create/open | `AppDatabase(executor)`, Drift schema v11. |
| File | `aveli_<userId>.sqlite`. |
| Login/restore | `LocalDatabaseManager.openForUser(userId)`. |
| Logout | `closeCurrent()`, file preserved. |
| Delete profile | `deleteLocalDataForUser`. |
| Active provider | `appDatabaseProvider`; throws if DB unavailable/user mismatch. |
| Migration | Drift `MigrationStrategy` on open. |
| Tests | `NativeDatabase.memory()` for DB tests. |

## Legacy Database

Legacy-database helpers exist, but no production UI invokes `claimLegacyIfPresent: true`.

Do not document automatic legacy adoption as current behavior.
