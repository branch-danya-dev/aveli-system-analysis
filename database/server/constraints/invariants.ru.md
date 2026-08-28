# Инварианты серверной базы данных

> Значимые для продукта инварианты, обеспечиваемые PostgreSQL.

| Инвариант | Способ обеспечения |
|---|---|
| Один регистрационный пробный период на пользователя | Частичный уникальный индекс на `access_grants(user_id)` для строк регистрационного пробного периода. |
| Один снимок подписки на право доступа для пользователя | UNIQUE `(user_id, entitlement_id)`. |
| Идемпотентность вебхуков и событий | UNIQUE `subscription_events.external_event_id`. |
| Корректный срок действия сессии | CHECK `auth_sessions.expires_at > created_at`. |
| Корректный срок действия права доступа | CHECK для `starts_at` / `ends_at`. |
| Пожизненный доступ не имеет даты окончания | CHECK по типу права доступа и `ends_at`. |

## Граница ответственности

Документ содержит инварианты, **обеспечиваемые серверной PostgreSQL**.

Проверки на уровне приложения и ограничения локальной SQLite принадлежат своим каноническим документам и не должны смешиваться с набором серверных ограничений.

## Связанная документация

- [`../entities/access_grants.ru.md`](../entities/access_grants.ru.md)
- [`../entities/auth_sessions.ru.md`](../entities/auth_sessions.ru.md)
- [`../entities/subscriptions.ru.md`](../entities/subscriptions.ru.md)
- [`../entities/subscription_events.ru.md`](../entities/subscription_events.ru.md)
