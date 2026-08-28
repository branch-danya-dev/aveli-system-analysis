# Subscription Synchronization

> Backend reconciliation authenticated Aveli account с RevenueCat REST state.

## Trigger

Canonical endpoint:

```text
POST /v1/billing/sync
```

Normal client вызывает его после purchase или restore через RevenueCat SDK.

## Trust Model

Client не отправляет authoritative entitlement state.

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

## RevenueCat Result Handling

| RevenueCat result | Backend behavior |
|---|---|
| `ok` | Upsert normalized snapshot и resolve access. |
| `not_found` | Upsert `status=expired`, затем resolve access. |
| `unavailable` | Fail closed: `502 BILLING_SYNC_FAILED`. |

## Fail-Closed Semantics

`unavailable` не считается successful sync.

Implementation description прямо указывает: unverified stale active result не должен оставаться результатом этой sync attempt.

```text
RevenueCat unavailable
      ↓
verification not possible
      ↓
do not report successful synchronized entitlement
      ↓
502 BILLING_SYNC_FAILED
```

Это отличается от client offline grace, где используется previously verified secure snapshot.

## Entitlement

Canonical entitlement id: `support`.

## Persistence

[`../../database/server/entities/subscriptions.ru.md`](../../database/server/entities/subscriptions.ru.md)

## Access Integration

После successful reconciliation backend возвращает тот же access contract, что `GET /v1/access`.

Canonical decision:

[`../access/access-resolution.ru.md`](../access/access-resolution.ru.md)
