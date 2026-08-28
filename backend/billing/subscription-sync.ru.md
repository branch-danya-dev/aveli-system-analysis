# Subscription Synchronization

> Backend reconciliation authenticated Aveli account с RevenueCat REST state.

## Trigger

Canonical endpoint:

```text
POST /v1/billing/sync
```

Normal client вызывает его после purchase или restore через RevenueCat SDK.

## Trust Model

Client не отправляет:

```text
isPremium = true
entitlementStatus = active
```

Вместо этого:

```text
JWT sub
   ↓
server userId
   ↓
RevenueCat REST
GET /v1/subscribers/{userId}
```

Backend независимо проверяет subscription evidence.

## Flow

```text
authenticated userId
      ↓
RevenueCat subscriber lookup
      ↓
map entitlements.support
      ↓
normalize SubscriptionStatus
      ↓
upsert subscriptions(user_id, entitlement_id)
      ↓
run common access resolution
      ↓
return AccessStatusView
```

## Result Handling

| RevenueCat result | Backend behavior |
|---|---|
| `ok` | Upsert normalized snapshot. |
| `not_found` | Upsert snapshot с `status=expired`. |
| `unavailable` | Fail closed: `502 BILLING_SYNC_FAILED`. |

При RevenueCat unavailable sync request не должен возвращать successful access на основании client assertion.

## Entitlement

Canonical entitlement id:

```text
support
```

## Persistence

Snapshot table:

[`../../database/server/entities/subscriptions.ru.md`](../../database/server/entities/subscriptions.ru.md)

## Access Integration

После sync backend возвращает тот же effective result, что `GET /v1/access`.

Canonical access decision:

[`../access/access-resolution.ru.md`](../access/access-resolution.ru.md)
