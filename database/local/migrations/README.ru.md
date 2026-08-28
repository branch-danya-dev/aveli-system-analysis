# Local SQLite Migrations

> История Drift schema per-user workspace database.

Текущая schema version: **11**

| Version | Изменение |
|---|---|
| 1–2 | Начальная schema (`createAll`). |
| 3 | `payments.method`. |
| 4 | `visit_photos.client_id`, `type`; rename `path` → `local_path`. |
| 5 | Один payment на appointment; CASCADE при delete appointment. |
| 6 | `payments.amount_paid`, `payments.paid_at`. |
| 7 | Rebuild `visit_notes` / `visit_photos` с CASCADE. |
| 8 | Repair `visit_photos.client_id` через join с appointments. |
| 9 | `app_settings` под управлением Drift. |
| 10 | `clients.device_contact_id`. |
| 11 | `clients.tags`, `clients.archived_at`. |

Canonical implementation source:

`lib/core/database/app_database.dart`
