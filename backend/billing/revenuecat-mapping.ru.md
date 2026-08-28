# RevenueCat → Subscription Snapshot Mapping

> Canonical backend normalization RevenueCat signals в `SubscriptionStatus`.

## Entitlement

```text
support
```

## Mapping

| RevenueCat signal | Persisted `SubscriptionStatus` |
|---|---|
| No entitlement | `expired` |
| `refunded_at` | `revoked` |
| Active grace period | `grace_period` |
| `period_type=trial` | `trialing` |
| `billing_issues_detected_at` | `past_due` |
| `unsubscribe_detected_at` | `cancelled`, `autoRenew=false` |
| Expiration is in the future | `active` |
| Otherwise | `expired` |

## Separation from Access

Mapping subscription state не равен access decision.

Например:

```text
status = cancelled
current_period_end > now
```

все еще участвует как entitled subscription согласно access predicate.

Canonical access logic:

[`../access/access-resolution.ru.md`](../access/access-resolution.ru.md)

## Contract Stability

Mapper heuristics — backend-internal behavior.

Они могут меняться без Flutter breaking change, если public `AccessStatusView` contract и product behavior остаются compatible.
