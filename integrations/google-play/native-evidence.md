# Google Play — Native Evidence

## Verified Native Identity

```text
application id: com.aveli.aveli
compileSdk: 37
minSdk: flutter.minSdkVersion
targetSdk: flutter.targetSdkVersion
```

The exact numeric min/target values were not established by the evidence document.

## RevenueCat

Android billing is reached through:

```text
purchases_flutter
```

## Store Management

Aveli uses:

```text
https://play.google.com/store/account/subscriptions?package=com.aveli.aveli
```

with optional `sku` when available.

## Product / Base Plan Evidence

Production Play product ids, base plans, offers, and RevenueCat-to-Play linking are **OPEN** because Play Console evidence is not present.

Names found only in test fixtures must not be treated as verified production ids.

## Android Manifest / Build Evidence

Verified:

- notification boot receiver;
- scheduled notification receiver;
- core library desugaring enabled for notification support.

No explicit billing permission is declared in the inspected manifest.

No repository `proguard-rules.pro` evidence was found for billing-specific rules.

## Canonical Related Docs

- [`../revenuecat/mobile-sdk.md`](../revenuecat/mobile-sdk.md)
- [`../../frontend/billing/`](../../frontend/billing/)
