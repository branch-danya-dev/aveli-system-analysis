# Google Play — Native Evidence

## Verified Native Identity

```text
application id: com.aveli.aveli
compileSdk: 37
minSdk: flutter.minSdkVersion
targetSdk: flutter.targetSdkVersion
```

Exact numeric min/target values evidence document не установил.

## RevenueCat

Android billing идет через:

```text
purchases_flutter
```

## Store Management

Aveli использует:

```text
https://play.google.com/store/account/subscriptions?package=com.aveli.aveli
```

с optional `sku`.

## Product / Base Plan Evidence

Production Play product ids, base plans, offers и RevenueCat-to-Play linking — **OPEN**, потому что Play Console evidence отсутствует.

Названия только из test fixtures не считаются verified production ids.

## Android Manifest / Build Evidence

Verified:

- notification boot receiver;
- scheduled notification receiver;
- core library desugaring для notifications.

Explicit billing permission в inspected manifest не declared.

Billing-specific `proguard-rules.pro` в repository не найден.

## Canonical Related Docs

- [`../revenuecat/mobile-sdk.ru.md`](../revenuecat/mobile-sdk.ru.md)
- [`../../frontend/billing/`](../../frontend/billing/)
