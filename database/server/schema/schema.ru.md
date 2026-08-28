# Aveli — физическая схема серверной PostgreSQL

## Таблицы

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

## Связи

```text
users 1 → 0..* auth_sessions          ON DELETE CASCADE
users 1 → 0..* access_grants          ON DELETE CASCADE
users 1 → 0..* subscriptions          ON DELETE CASCADE
users 1 → 0..* subscription_events    ON DELETE SET NULL
subscriptions 1 → 0..* subscription_events ON DELETE SET NULL
```

## Критичные для продукта правила хранения

- один регистрационный пробный период на пользователя обеспечивается частичным уникальным индексом;
- одна строка подписки на `(user_id, entitlement_id)` обеспечивается ограничением UNIQUE;
- идемпотентность вебхуков поддерживается ограничением уникальности `subscription_events.external_event_id`;
- срок действия сессии должен удовлетворять `expires_at > created_at`;
- временные границы права доступа ограничены проверочными ограничениями;
- регистрационный пробный период и состояние подписки физически разделены.

## Сопоставление источников доступа с хранением

| Источник доступа | Физический источник |
|---|---|
| Lifetime | `access_grants`, `type=lifetime` |
| Manual | `access_grants`, `type=manual_temporary` |
| Подписка | `subscriptions` |
| Регистрационный пробный период | `access_grants`, `type=trial`, `source=registration` |

Алгоритм определения доступа относится к `backend/access`; этот документ содержит только факты хранения, которыми пользуется алгоритм.
