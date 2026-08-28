# `auth_sessions`

> Сохранённые записи сессий обновления токена.

| Поле | Тип | Назначение |
|---|---|---|
| `id` | UUID PK | Идентификатор сессии. |
| `user_id` | UUID FK | Пользователь-владелец; → `users`, ON DELETE CASCADE. |
| `refresh_token_hash` | TEXT | Хеш токена обновления; сам токен в открытом виде не хранится. |
| `device_id` | TEXT, допускает NULL | Идентификатор устройства, если известен. |
| `device_name` | TEXT, допускает NULL | Отображаемое имя устройства. |
| `platform` | TEXT, допускает NULL | Платформа: `android`, `ios` или другое значение. |
| `created_at` | TIMESTAMPTZ | Время создания сессии. |
| `last_used_at` | TIMESTAMPTZ | Последнее использование сессии. |
| `expires_at` | TIMESTAMPTZ | Граница срока действия. |
| `revoked_at` | TIMESTAMPTZ, допускает NULL | Время отзыва при выходе или ротации. |

## Ограничения и индексы

- CHECK: `expires_at > created_at`
- индекс: `user_id`
- индекс: `expires_at`

## Связанная документация

- [`../constraints/invariants.ru.md`](../constraints/invariants.ru.md)
