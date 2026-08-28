# `subscriptions`

> RevenueCat entitlement-state snapshot persisted by Aveli.

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

`trialing` may exist as a store/provider subscription status. The Aveli registration trial is not stored here; it lives in `access_grants`.

This table is a denormalized provider-state snapshot, not event sourcing.
