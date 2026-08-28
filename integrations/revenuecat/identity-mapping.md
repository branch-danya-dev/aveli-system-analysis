# RevenueCat Identity Mapping

## Canonical Chain

```text
PostgreSQL users.id (UUID)
  → Flutter AuthSession.user.id
  → SecureTokenStorage.aveli_user_id
  → LocalDatabaseManager aveli_<userId>.sqlite
  → Purchases.logIn(userId)
  → RevenueCat App User ID
  → GET /v1/subscribers/{userId}
  → webhook event.app_user_id
  → subscriptions.user_id
```

The Aveli server UUID is the cross-system identity anchor.

## Login Timing

RevenueCat login occurs after the authenticated user workspace is activated.

## Logout / Account Switch

Current client:

```text
RevenueCat logOut
  ↓
clear Aveli session
  ↓
close current DB
  ↓
new Aveli login
  ↓
RevenueCat logIn(newUserId)
```

## Server Protection

Billing sync always keys RevenueCat lookup from authenticated JWT `userId`.

The client cannot choose another RevenueCat account id for the sync endpoint.

## Webhook Protection

`app_user_id` must match expected UUID format before reconciliation.

## Open Provider Semantics

The current repository does not establish:

- RevenueCat anonymous-to-identified merge details;
- dashboard alias/transfer behavior;
- provider-side customer transfer rules.

These remain OPEN until RevenueCat project/dashboard evidence is available.
