# `clients`

> Physical client records в local workspace.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK; UUID v4. |
| `name` | TEXT | NO | Отображаемое имя. |
| `phone` | TEXT | YES | Телефон. |
| `social` | TEXT | YES | Соцсеть / мессенджер. |
| `device_contact_id` | TEXT | YES | Stable device-contact identifier; v10. |
| `tags` | TEXT | YES | JSON array строк; v11. |
| `archived_at` | DATETIME | YES | Timestamp архивации; v11. |
| `created_at` | DATETIME | NO | Timestamp создания. |

## Constraints и Lifecycle

Документирован только PK index.

Application delete блокируется, если у client есть appointments. Предпочтительный lifecycle path — archive через `archived_at`.

Physical behavior должно оставаться traceable к финальным client lifecycle rules.
