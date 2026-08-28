# `visit_notes`

> Physical visit-note records.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK; UUID. |
| `appointment_id` | TEXT | NO | FK → `appointments.id`; ON DELETE CASCADE. |
| `body` | TEXT | NO | Текст заметки. |
| `created_at` | DATETIME | NO | Создание. |
