# Mobile Purchase / Restore Flow

## RevenueCat Service

```text
RevenueCatPurchaseService
```

`NoopPurchaseService` используется, если mobile RevenueCat keys пусты.

## Configuration

Dart defines:

```text
REVENUECAT_IOS_API_KEY
REVENUECAT_ANDROID_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Default entitlement:

```text
support
```

Product ids не hardcoded во Flutter; загружаются из RevenueCat/store offerings.

## Customer Identity

После authenticated workspace activation:

```text
Purchases.logIn(userId)
```

где `userId` — server UUID.

## Purchase

```text
Paywall
  ↓
Purchases.purchase
  ↓
purchase result
  ↓
AccessController.syncBilling
  ↓
POST /v1/billing/sync
  ↓
server AccessStatusView
  ↓
AccessState + secure snapshot
```

## Restore

```text
Purchases.restorePurchases
  ↓
syncBilling
  ↓
server access state
```

## Authority Boundary

Client может читать `CustomerInfo.entitlements[support].isActive` как часть purchase outcome, но это не direct workspace unlock.

Workspace unlock идет через backend reconciliation.

## Listener Model

Global `addCustomerInfoUpdateListener` отсутствует.

На app resume client один раз получает current customer info и refreshes/syncs access.

## Purchase Result States

```text
success
syncPending
pending
cancelled
```
