# Local Database Lifecycle

## Canonical Physical Schema

См.:

[`../../database/local/`](../../database/local/)

Этот документ владеет Flutter-side lifecycle usage.

## Verified Behavior

| Concern | Frontend behavior |
|---|---|
| Create/open | `AppDatabase(executor)`, Drift schema v11. |
| File | `aveli_<userId>.sqlite`. |
| Login/restore | `LocalDatabaseManager.openForUser(userId)`. |
| Logout | `closeCurrent()`, file preserved. |
| Delete profile | `deleteLocalDataForUser`. |
| Active provider | `appDatabaseProvider`; throws при DB unavailable/user mismatch. |
| Migration | Drift `MigrationStrategy` on open. |
| Tests | `NativeDatabase.memory()` для DB tests. |

## Legacy Database

Legacy helpers существуют, но production UI не вызывает `claimLegacyIfPresent: true`.

Automatic legacy adoption не является current behavior.
