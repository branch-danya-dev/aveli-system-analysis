# `services`

> Physical service records in the local workspace.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK; UUID v4. |
| `name` | TEXT | NO | Service name. |
| `duration` | INTEGER | NO | Duration in minutes. |
| `price` | INTEGER | NO | Price in whole profile-currency units. |
| `return_interval` | INTEGER | YES | Return interval in minutes. |

## Verification Note

The source ER overview uses `duration_minutes` / `return_interval_minutes`, while the detailed table definition uses `duration` / `return_interval`. This document follows the detailed definition pending direct code verification.
