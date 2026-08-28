# `visit_photos`

> Metadata visit-photo files.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK; UUID. |
| `appointment_id` | TEXT | NO | FK → `appointments.id`; ON DELETE CASCADE. |
| `client_id` | TEXT | NO | FK → `clients.id`; denormalized client reference. |
| `local_path` | TEXT | NO | Абсолютный local file path. |
| `type` | TEXT | NO | `VisitPhotoType`: before / after / general. |
| `created_at` | DATETIME | NO | Создание. |

## Physical File Boundary

Строка хранит metadata. Байты изображения находятся в device file storage.

`client_id` намеренно синхронизируется со связью appointment; в этой модели участвуют migrations v4, v7 и v8.
