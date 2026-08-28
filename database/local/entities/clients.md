# `clients`

> Physical client records in the local workspace.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK; UUID v4. |
| `name` | TEXT | NO | Display name. |
| `phone` | TEXT | YES | Phone. |
| `social` | TEXT | YES | Social network / messenger. |
| `device_contact_id` | TEXT | YES | Stable device-contact identifier; added in v10. |
| `tags` | TEXT | YES | JSON array of strings; added in v11. |
| `archived_at` | DATETIME | YES | Archive timestamp; added in v11. |
| `created_at` | DATETIME | NO | Creation timestamp. |

## Constraints and Lifecycle

Only the PK index is documented.

Application deletion is blocked when the client has appointments. The preferred lifecycle path is archive through `archived_at`.

This physical behavior should remain traceable to the finalized client lifecycle rules.
