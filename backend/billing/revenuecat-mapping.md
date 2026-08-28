# RevenueCat → Subscription Snapshot Mapping

> Canonical backend normalization of RevenueCat signals into `SubscriptionStatus`.

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

Mapping subscription state is not the same as deciding access.

For example:

```text
status = cancelled
current_period_end > now
```

still participates as an entitled subscription according to the access predicate.

Canonical access logic:

[`../access/access-resolution.md`](../access/access-resolution.md)

## Contract Stability

The mapper heuristics are backend-internal behavior.

They may change without a Flutter breaking change as long as the public `AccessStatusView` contract and product behavior remain compatible.
