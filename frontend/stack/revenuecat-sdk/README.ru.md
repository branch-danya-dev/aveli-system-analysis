# RevenueCat Mobile SDK

> Mobile purchase/restore integration через `purchases_flutter`.

## Verified Version

`purchases_flutter 10.9.1`

## Role

Client загружает offerings, запускает purchase/restore, выполняет logIn server UUID как RevenueCat App User ID и затем вызывает backend `/v1/billing/sync`.

RevenueCat client state не final access authority.

## Configuration

Public dart-defines:

- `REVENUECAT_IOS_API_KEY`
- `REVENUECAT_ANDROID_API_KEY`
- `REVENUECAT_ENTITLEMENT_ID` (default `support`)

## Replaceability

**Medium.** Usage скрыт за `PurchaseService`, поэтому impact в основном ограничен billing/subscription UI.
