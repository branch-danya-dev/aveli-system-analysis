# Subscription Synchronization

> Backend reconciliation of the authenticated Aveli account with RevenueCat REST state.

## Trigger

Canonical endpoint:

```text
POST /v1/billing/sync
```

The normal client triggers it after purchase or restore in the RevenueCat SDK.

## Trust Model

The client does not send:

```text
isPremium = true
entitlementStatus = active
```

Instead:

```text
JWT sub
   ↓
server userId
   ↓
RevenueCat REST
GET /v1/subscribers/{userId}
```

The backend therefore verifies subscription evidence independently of client-declared entitlement state.

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
| `not_found` | Upsert snapshot with `status=expired`. |
| `unavailable` | Fail closed with `502 BILLING_SYNC_FAILED`. |

When RevenueCat is unavailable, the sync request must not return successful access based purely on an unverified client assertion.

## Entitlement

Canonical entitlement id:

```text
support
```

## Persistence

Snapshot table:

[`../../database/server/entities/subscriptions.md`](../../database/server/entities/subscriptions.md)

## Access Integration

After synchronization the backend returns the same effective result as `GET /v1/access`.

Canonical access decision:

[`../access/access-resolution.md`](../access/access-resolution.md)
