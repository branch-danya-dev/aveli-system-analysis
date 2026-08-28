# `subscriptions`

> RevenueCat entitlement-state snapshot, сохраняемый Aveli.

| Field | Type | Default | Meaning |
|---|---|---|---|
| `id` | UUID PK | — | Идентификатор subscription row. |
| `user_id` | UUID FK | — | Owning user; ON DELETE CASCADE. |
| `provider` | SubscriptionProvider | `revenuecat` | Subscription provider. |
| `entitlement_id` | TEXT | `support` | Текущий logical RevenueCat entitlement. |
| `product_id` | TEXT | — | Store product identifier. |
| `store` | Store | `unknown` | `app_store`, `play_store` или `unknown`. |
| `status` | SubscriptionStatus | — | Текущий provider-backed subscription state. |
| `auto_renew` | BOOLEAN | — | Auto-renew state. |
| `purchased_at` | TIMESTAMPTZ nullable | — | Время покупки. |
| `current_period_start` | TIMESTAMPTZ nullable | — | Начало текущего period. |
| `current_period_end` | TIMESTAMPTZ nullable | — | Конец текущего period. |
| `provider_customer_id` | TEXT nullable | — | RevenueCat app user/customer identifier. |
| `original_transaction_id` | TEXT nullable | — | Original store transaction identifier. |
| `last_verified_at` | TIMESTAMPTZ nullable | — | Время последней verification. |
| `created_at` | TIMESTAMPTZ | — | Время создания. |
| `updated_at` | TIMESTAMPTZ | — | Время обновления. |

## Constraints

UNIQUE:

```text
(user_id, entitlement_id)
```

На одного user хранится одна subscription snapshot row на entitlement.

## Индексы

```text
user_id
status
entitlement_id
```

## Trial Boundary

`trialing` может существовать как store/provider subscription status.

Aveli registration trial **не** хранится в этой table. Его persistence принадлежит `access_grants`.

## Persistence Meaning

Table является denormalized provider-state snapshot, а не event-sourcing storage.

Raw provider events хранятся отдельно в `subscription_events`.

## Связанная документация

- [`access_grants.ru.md`](access_grants.ru.md)
- [`subscription_events.ru.md`](subscription_events.ru.md)
- [`../schema/enums.ru.md`](../schema/enums.ru.md)
