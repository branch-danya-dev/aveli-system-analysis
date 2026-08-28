# Apple App Store — Native Evidence

## Verified Native Identity

```text
bundle identifier: com.aveli.aveli
deployment target: iOS 15.0
```

RevenueCat client integration is delivered through:

```text
purchases_flutter
```

## Native Permission Evidence

`Info.plist` includes usage descriptions for:

- camera;
- photo library;
- contacts.

## Subscription Management

Aveli can open Apple's subscription-management URL:

```text
https://apps.apple.com/account/subscriptions
```

through the client handoff layer.

## Product Configuration

Production product ids, App Store subscription group, and App Store Connect offering structure are **OPEN**.

The repository contains product names such as:

```text
aveli_support_monthly
aveli_support_yearly
```

only in backend test fixtures.

They must not be presented as verified production App Store product identifiers.

## StoreKit Project Evidence

No repository evidence establishes:

- `.storekit` configuration;
- App Store Connect subscription group;
- production product metadata;
- real-store E2E tests.

No dedicated IAP `.entitlements` evidence was found in the inspected repository.

## Pricing Boundary

Aveli UI uses provider/store localized price data through RevenueCat.

Hardcoded test fixture ids/prices are not authoritative production store configuration.

## Canonical Related Docs

- [`../revenuecat/mobile-sdk.md`](../revenuecat/mobile-sdk.md)
- [`../../frontend/billing/`](../../frontend/billing/)
