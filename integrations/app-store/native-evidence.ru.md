# Apple App Store — Native Evidence

## Verified Native Identity

```text
bundle identifier: com.aveli.aveli
deployment target: iOS 15.0
```

RevenueCat client integration идет через:

```text
purchases_flutter
```

## Native Permission Evidence

`Info.plist` содержит usage descriptions для:

- camera;
- photo library;
- contacts.

## Subscription Management

Aveli может открыть Apple subscription-management URL:

```text
https://apps.apple.com/account/subscriptions
```

через client handoff layer.

## Product Configuration

Production product ids, App Store subscription group и App Store Connect offering structure — **OPEN**.

Repository содержит:

```text
aveli_support_monthly
aveli_support_yearly
```

только в backend test fixtures.

Их нельзя выдавать за verified production App Store product ids.

## StoreKit Project Evidence

Repository не подтверждает:

- `.storekit` configuration;
- App Store Connect subscription group;
- production product metadata;
- real-store E2E tests.

Dedicated IAP `.entitlements` evidence в inspected repo не найден.

## Pricing Boundary

Aveli UI использует provider/store localized price data через RevenueCat.

Hardcoded test fixture ids/prices не authoritative production store config.

## Canonical Related Docs

- [`../revenuecat/mobile-sdk.ru.md`](../revenuecat/mobile-sdk.ru.md)
- [`../../frontend/billing/`](../../frontend/billing/)
