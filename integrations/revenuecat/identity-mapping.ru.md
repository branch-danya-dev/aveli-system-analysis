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

Aveli server UUID — cross-system identity anchor.

## Login Timing

RevenueCat login выполняется после activation authenticated user workspace.

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

Billing sync всегда использует authenticated JWT `userId` для RevenueCat lookup.

Client не может выбрать другой RevenueCat account id для sync endpoint.

## Webhook Protection

`app_user_id` должен соответствовать expected UUID format до reconciliation.

## Open Provider Semantics

Repository не подтверждает:

- RevenueCat anonymous-to-identified merge details;
- dashboard alias/transfer behavior;
- provider-side customer transfer rules.

Они остаются OPEN до RevenueCat dashboard evidence.
