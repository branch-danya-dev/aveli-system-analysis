# Aveli — Local SQLite Physical Schema

## Файлы БД

| Путь | Назначение |
|---|---|
| `documents/aveli_<userId>.sqlite` | Активная per-user professional workspace database. |
| `documents/aveli.db` | Legacy single-file database; автоматически к account не подключается. |
| `documents/visit_photos/<sanitizedUserId>/` | Binary visit-photo files вне SQLite. |

`LocalDatabaseManager.openForUser(userId)` открывает или создает per-user database.

Legacy `aveli.db` может быть присвоена пользователю только через explicit legacy-claim path. Закрытие workspace закрывает connection, но не удаляет файл. Explicit local-data deletion удаляет per-user SQLite file.

Local entity identifiers — UUID v4, генерируемые application use cases.

## Таблицы

```text
clients
services
appointments
payments
visit_notes
visit_photos
app_settings
```

## Связи

```text
clients      1 ── 0..* appointments
services     1 ── 0..* appointments
appointments 1 ── 0..1 payments
appointments 1 ── 0..* visit_notes
appointments 1 ── 0..* visit_photos
clients      1 ── 0..* visit_photos
```

Подтвержденный invariant:

> `payments.appointment_id` имеет UNIQUE, поэтому physical appointment имеет максимум одну payment row.

Partial payment хранится внутри этой строки через `amount_paid` и `status=partial`.

## Намеренная денормализация

Schema намеренно дублирует некоторые значения:

- `appointments.price` фиксирует snapshot цены услуги на момент записи;
- `appointments.payment_status` хранит агрегированный UI-oriented payment state;
- `visit_photos.client_id` дублирует связь с client, которую также можно получить через appointment.

## Индексы

Известные non-PK indexes:

```text
appointments_starts_at(starts_at)
appointments_client_id(client_id)
```

Остальные table-specific indexes описываются рядом с entities.

## Замечание о consistency источника

В предоставленном persistence-документе есть одно расхождение имен:

```text
ER overview:      duration_minutes / return_interval_minutes
Detailed table:  duration / return_interval
```

В этой документации используются имена из detailed table, но exact SQL column names стоит повторно сверить с `lib/core/database/tables/*.dart` до фиксации generated SQL как canonical.

## Связанная документация

- [`../entities/`](../entities/)
- [`../migrations/`](../migrations/)
- [`../files/visit-photos.ru.md`](../files/visit-photos.ru.md)
