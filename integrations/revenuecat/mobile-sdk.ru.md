# RevenueCat — Mobile SDK Boundary

## Technology

```text
purchases_flutter 10.9.1
```

Main client class:

```text
RevenueCatPurchaseService
```

## Initialization

RevenueCat configuration lazy.

`ensureConfigured()` вызывается при первом:

```text
logIn
getOfferings
purchase
restore
refreshCustomerInfo
```

Cold-start bootstrap не выполняет eager initialization.

Если обе platform public keys пусты, frontend использует:

```text
NoopPurchaseService
```

## Public Configuration

```text
REVENUECAT_IOS_API_KEY
REVENUECAT_ANDROID_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Default entitlement:

```text
support
```

Mobile SDK keys — public application configuration, не backend secrets.

## Offerings and Products

Production product ids не hardcoded во Flutter.

Client загружает:

```text
Purchases.getOfferings()
→ current.availablePackages
```

Monthly/annual presentation выводится из RevenueCat package/product metadata.

Authoritative store price отображается из provider product data:

```text
storeProduct.priceString
storeProduct.price
```

## Purchase Flow

```text
Paywall
  ↓
Purchases.purchase(...)
  ↓
CustomerInfo / purchase outcome
  ↓
AccessController.syncBilling
  ↓
POST /v1/billing/sync
  ↓
Backend AccessStatusView
```

RevenueCat client entitlement state сам по себе не unlocks workspace.

## Restore Flow

```text
Purchases.restorePurchases()
  ↓
syncBilling
  ↓
backend access state
```

## Resume Behavior

Global `addCustomerInfoUpdateListener` отсутствует.

На app resume frontend явно refreshes RevenueCat customer info и затем refreshes/synchronizes access.

## Verified Outcome States

```text
success
cancelled
pending
error
unavailable
```

Component implementation:

[`../../frontend/billing/purchase-flow.ru.md`](../../frontend/billing/purchase-flow.ru.md)
