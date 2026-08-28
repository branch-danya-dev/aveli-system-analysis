# RevenueCat Mobile SDK

> Mobile purchase/restore integration implemented through `purchases_flutter`.

## Verified Version

`purchases_flutter 10.9.1`

## Role

The client loads offerings, starts purchase/restore, logs in with server UUID as RevenueCat App User ID, and then triggers backend `/v1/billing/sync`.

RevenueCat client state is not the final access authority.

## Configuration

Public dart-defines:

- `REVENUECAT_IOS_API_KEY`
- `REVENUECAT_ANDROID_API_KEY`
- `REVENUECAT_ENTITLEMENT_ID` (default `support`)

## Replaceability

**Medium.** Usage is hidden behind `PurchaseService`, reducing impact outside billing/subscription UI.
