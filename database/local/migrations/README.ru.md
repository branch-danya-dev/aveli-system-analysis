# Миграции локального SQLite

> История схемы Drift для пользовательской базы данных рабочего пространства.

Текущая версия схемы: **11**.

| Версия | Изменение |
|---|---|
| 1–2 | Начальная схема (`createAll`). |
| 3 | Добавлено `payments.method`. |
| 4 | Добавлены `visit_photos.client_id`, `type`; `path` переименован в `local_path`. |
| 5 | Ограничение одной оплаты на запись; каскадное удаление при удалении записи. |
| 6 | Добавлены `payments.amount_paid`, `payments.paid_at`. |
| 7 | Перестроены `visit_notes` / `visit_photos` с каскадным удалением. |
| 8 | Восстановление `visit_photos.client_id` через связь с `appointments`. |
| 9 | `app_settings` переведена под управление Drift. |
| 10 | Добавлен `clients.device_contact_id`. |
| 11 | Добавлены `clients.tags`, `clients.archived_at`. |

Канонический источник реализации:

`lib/core/database/app_database.dart`
