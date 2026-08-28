# Mobile Purchase / Restore Flow

## RevenueCat Service

```text
RevenueCatPurchaseService
```

A `NoopPurchaseService` is used when mobile RevenueCat keys are empty.

## Configuration

Dart defines:

```text
REVENUECAT_IOS_API_KEY
REVENUECAT_ANDROID_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Default entitlement id:

```text
support
```

Product ids are not hardcoded in Flutter; they are loaded from RevenueCat/store offerings.

## Customer Identity

After authenticated workspace activation:

```text
Purchases.logIn(userId)
```

where `userId` is the server UUID.

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

The client may inspect `CustomerInfo.entitlements[support].isActive` as part of purchase outcome handling, but that state does not directly unlock the workspace.

Workspace unlock goes through backend reconciliation.

## Listener Model

There is no global `addCustomerInfoUpdateListener`.

On app resume, the client fetches current customer info once and then refreshes/syncs access.

## Purchase Result States

Verified client result model includes:

```text
success
syncPending
pending
cancelled
```
