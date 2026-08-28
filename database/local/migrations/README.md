# Local SQLite Migrations

> Drift schema history for the per-user workspace database.

Current schema version: **11**

| Version | Change |
|---|---|
| 1–2 | Initial schema (`createAll`). |
| 3 | Add `payments.method`. |
| 4 | Add `visit_photos.client_id`, `type`; rename `path` → `local_path`. |
| 5 | Enforce one payment per appointment; add CASCADE on appointment deletion. |
| 6 | Add `payments.amount_paid`, `payments.paid_at`. |
| 7 | Rebuild `visit_notes` / `visit_photos` with CASCADE. |
| 8 | Repair `visit_photos.client_id` through appointments join. |
| 9 | Bring `app_settings` under Drift management. |
| 10 | Add `clients.device_contact_id`. |
| 11 | Add `clients.tags`, `clients.archived_at`. |

Canonical implementation source:

`lib/core/database/app_database.dart`
