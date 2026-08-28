# Local Persistence

> Проверенная persistence model профессионального workspace Aveli на устройстве пользователя.

## Назначение

`local/` описывает physical persistence, принадлежащий device-side workspace.

Текущая проверенная модель:

```text
Per-user SQLite database
+
Visit-photo files
+
Secure client-side access/session state
```

## Основная граница

Для каждого authenticated user открывается отдельная SQLite database:

```text
documents/aveli_<userId>.sqlite
```

Server user UUID используется для выбора файла БД и пути к фото, но local business entities **не имеют** foreign keys на server table `users`.

Professional workspace data не синхронизируются с backend.

## Навигация

| Область | Ответственность |
|---|---|
| `schema/` | Текущая SQLite schema и physical relationships. |
| `entities/` | Table-level physical documentation. |
| `migrations/` | История Drift schema. |
| `files/` | Persistence файлов visit photos. |
| `secure-storage/` | Persistence boundary для access snapshot и auth tokens вне SQLite. |

## Проверенные implementation sources

- `lib/core/database/app_database.dart`
- `lib/core/database/tables/*.dart`
- `lib/core/database/local_database_manager.dart`
