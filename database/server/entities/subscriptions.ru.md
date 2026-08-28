# `subscriptions`

> RevenueCat entitlement-state snapshot, сохраняемый Aveli.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Subscription-row identifier. |
| `user_id` | UUID FK | → users; ON DELETE CASCADE. |
| `provider` | SubscriptionProvider | Default revenuecat. |
| `entitlement_id` | TEXT | Current logical entitlement; `support`. |
| `product_id` | TEXT | Store product id. |
| `store` | Store | app_store / play_store / unknown. |
| `status` | SubscriptionStatus | Current snapshot status. |
| `auto_renew` | BOOLEAN | Auto-renew flag. |
| `purchased_at` | TIMESTAMPTZ nullable | Purchase time. |
| `current_period_start` | TIMESTAMPTZ nullable | Current period start. |
| `current_period_end` | TIMESTAMPTZ nullable | Current period end. |
| `provider_customer_id` | TEXT nullable | RevenueCat app user id. |
| `original_transaction_id` | TEXT nullable | Original transaction id. |
| `last_verified_at` | TIMESTAMPTZ nullable | Last verification. |
| `created_at` | TIMESTAMPTZ | Creation. |
| `updated_at` | TIMESTAMPTZ | Update. |

## Constraints / Meaning

UNIQUE:

```text
(user_id, entitlement_id)
```

Current entitlement id:

```text
support
```

`trialing` может существовать как store/provider subscription status. Aveli registration trial здесь не хранится; он находится в `access_grants`.

Table — denormalized provider-state snapshot, а не event sourcing.
