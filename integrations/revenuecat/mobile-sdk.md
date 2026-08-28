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

RevenueCat configuration is lazy.

`ensureConfigured()` runs on first:

```text
logIn
getOfferings
purchase
restore
refreshCustomerInfo
```

It is not eagerly initialized during cold-start bootstrap.

When both platform public keys are empty, the frontend uses:

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

These mobile SDK keys are public application configuration, not backend secrets.

## Offerings and Products

Production product ids are not hardcoded in Flutter.

The client loads:

```text
Purchases.getOfferings()
→ current.availablePackages
```

Monthly/annual presentation is inferred from RevenueCat package type/product metadata.

Authoritative store price display comes from provider product data:

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

RevenueCat client entitlement state alone does not unlock the workspace.

## Restore Flow

```text
Purchases.restorePurchases()
  ↓
syncBilling
  ↓
backend access state
```

## Resume Behavior

There is no global `addCustomerInfoUpdateListener`.

On app resume, the frontend explicitly refreshes RevenueCat customer info and then refreshes/synchronizes access.

## Verified Outcome States

```text
success
cancelled
pending
error
unavailable
```

Component implementation details:

[`../../frontend/billing/purchase-flow.md`](../../frontend/billing/purchase-flow.md)
