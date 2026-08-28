# Subscription Synchronization

> Backend reconciliation of the authenticated Aveli account with RevenueCat REST state.

## Trigger

Canonical endpoint:

```text
POST /v1/billing/sync
```

The normal client triggers it after purchase or restore in the RevenueCat SDK.

## Trust Model

The client does not send authoritative entitlement state.

Instead:

```text
JWT sub
   ↓
server userId
   ↓
RevenueCat REST
GET /v1/subscribers/{userId}
```

The backend verifies subscription evidence independently of client-declared purchase state.

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
| `ok` | Upsert normalized snapshot and resolve access. |
| `not_found` | Upsert `status=expired`, then resolve access. |
| `unavailable` | Fail closed with `502 BILLING_SYNC_FAILED`. |

## Fail-Closed Semantics

`unavailable` is not treated as a successful sync.

The implementation description explicitly states that an unverified stale active result must not remain the result of this sync attempt.

```text
RevenueCat unavailable
      ↓
verification not possible
      ↓
do not report successful synchronized entitlement
      ↓
502 BILLING_SYNC_FAILED
```

This is different from client offline grace, which relies on a previously verified secure snapshot.

## Entitlement

Canonical entitlement id: `support`.

## Persistence

[`../../database/server/entities/subscriptions.md`](../../database/server/entities/subscriptions.md)

## Access Integration

After successful reconciliation the backend returns the same access contract as `GET /v1/access`.

Canonical decision:

[`../access/access-resolution.md`](../access/access-resolution.md)
