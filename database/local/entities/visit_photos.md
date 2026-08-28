# `visit_photos`

> Metadata for visit-photo files.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK; UUID. |
| `appointment_id` | TEXT | NO | FK → `appointments.id`; ON DELETE CASCADE. |
| `client_id` | TEXT | NO | FK → `clients.id`; denormalized client reference. |
| `local_path` | TEXT | NO | Absolute local file path. |
| `type` | TEXT | NO | `VisitPhotoType`: before / after / general. |
| `created_at` | DATETIME | NO | Creation timestamp. |

## Physical File Boundary

The row stores metadata. Image bytes live in device file storage.

`client_id` is intentionally synchronized with the appointment relation; migrations v4, v7, and v8 participate in this model.
