# `visit_notes`

> Физические записи заметок визита.

| Столбец | Тип | NULL | Назначение |
|---|---|---|---|
| `id` | TEXT | NO | Первичный ключ; UUID. |
| `appointment_id` | TEXT | NO | Внешний ключ → `appointments.id`; `ON DELETE CASCADE`. |
| `body` | TEXT | NO | Текст заметки. |
| `created_at` | DATETIME | NO | Время создания. |
