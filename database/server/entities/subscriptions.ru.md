# `subscriptions`

> Снимок состояния права доступа RevenueCat, сохраняемый Aveli.

| Поле | Тип | По умолчанию | Назначение |
|---|---|---|---|
| `id` | UUID PK | — | Идентификатор записи. |
| `user_id` | UUID FK | — | Пользователь-владелец; ON DELETE CASCADE. |
| `provider` | `SubscriptionProvider` | `revenuecat` | Провайдер подписки. |
| `store` | `Store` | `unknown` | Магазин, связанный с подпиской. |
| `product_id` | TEXT | — | Идентификатор продукта в магазине. |
| `entitlement_id` | TEXT | — | Идентификатор права доступа RevenueCat. |
| `status` | `SubscriptionStatus` | — | Текущее состояние подписки, полученное от провайдера. |
| `auto_renew` | BOOLEAN | — | Состояние автоматического продления. |
| `current_period_started_at` | TIMESTAMPTZ, допускает NULL | — | Начало текущего периода. |
| `current_period_ends_at` | TIMESTAMPTZ, допускает NULL | — | Конец текущего периода. |
| `grace_period_ends_at` | TIMESTAMPTZ, допускает NULL | — | Конец льготного периода. |
| `cancelled_at` | TIMESTAMPTZ, допускает NULL | — | Время отмены. |
| `revoked_at` | TIMESTAMPTZ, допускает NULL | — | Время отзыва. |
| `original_transaction_id` | TEXT, допускает NULL | — | Исходный идентификатор транзакции магазина. |
| `last_verified_at` | TIMESTAMPTZ, допускает NULL | — | Время последней проверки. |
| `created_at` | TIMESTAMPTZ | — | Время создания. |
| `updated_at` | TIMESTAMPTZ | — | Время обновления. |

## Ограничения

UNIQUE:

```text
(user_id, entitlement_id)
```

Для одного пользователя хранится одна строка снимка подписки на каждое право доступа.

## Индексы

```text
user_id
status
entitlement_id
```

## Граница пробного периода

`trialing` может существовать как состояние подписки в магазине или у провайдера.

Регистрационный пробный период Aveli **не** хранится в этой таблице. Его хранение относится к `access_grants`.

## Назначение хранения

Таблица является денормализованным снимком состояния провайдера, а не хранилищем событий.

Исходные события провайдера хранятся отдельно в `subscription_events`.

## Связанная документация

- [`access_grants.ru.md`](access_grants.ru.md)
- [`subscription_events.ru.md`](subscription_events.ru.md)
- [`../schema/enums.ru.md`](../schema/enums.ru.md)
