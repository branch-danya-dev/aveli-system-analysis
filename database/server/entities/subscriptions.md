# `subscriptions`

> RevenueCat entitlement-state snapshot persisted by Aveli.

| Field | Type | Default | Meaning |
|---|---|---|---|
| `id` | UUID PK | — | Subscription-row identifier. |
| `user_id` | UUID FK | — | Owning user; ON DELETE CASCADE. |
| `provider` | SubscriptionProvider | `revenuecat` | Subscription provider. |
| `entitlement_id` | TEXT | `support` | Current logical RevenueCat entitlement. |
| `product_id` | TEXT | — | Store product identifier. |
| `store` | Store | `unknown` | `app_store`, `play_store`, or `unknown`. |
| `status` | SubscriptionStatus | — | Current provider-backed subscription state. |
| `auto_renew` | BOOLEAN | — | Auto-renew state. |
| `purchased_at` | TIMESTAMPTZ nullable | — | Purchase timestamp. |
| `current_period_start` | TIMESTAMPTZ nullable | — | Current period start. |
| `current_period_end` | TIMESTAMPTZ nullable | — | Current period end. |
| `provider_customer_id` | TEXT nullable | — | RevenueCat app user/customer identifier. |
| `original_transaction_id` | TEXT nullable | — | Original store transaction identifier. |
| `last_verified_at` | TIMESTAMPTZ nullable | — | Last verification timestamp. |
| `created_at` | TIMESTAMPTZ | — | Creation timestamp. |
| `updated_at` | TIMESTAMPTZ | — | Update timestamp. |

## Constraints

UNIQUE:

```text
(user_id, entitlement_id)
```

There is one subscription snapshot row per entitlement per user.

## Indexes

```text
user_id
status
entitlement_id
```

## Trial Boundary

`trialing` may exist as a store/provider subscription status.

The Aveli registration trial is **not** stored in this table. Registration trial persistence belongs to `access_grants`.

## Persistence Meaning

This table is a denormalized provider-state snapshot, not an event-sourcing store.

Raw provider events are retained separately in `subscription_events`.

## Related Documentation

- [`access_grants.md`](access_grants.md)
- [`subscription_events.md`](subscription_events.md)
- [`../schema/enums.md`](../schema/enums.md)
