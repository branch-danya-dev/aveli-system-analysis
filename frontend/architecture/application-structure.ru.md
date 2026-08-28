# Frontend Application Structure

## Verified Source Shape

```text
lib/
├── app/
├── core/
│   ├── config/
│   ├── database/
│   ├── design/
│   ├── design_system/
│   ├── domain/
│   ├── errors/
│   ├── l10n/
│   ├── providers/
│   └── widgets/
├── features/
│   ├── appointments/
│   ├── auth/
│   ├── bootstrap/
│   ├── calendar/
│   ├── clients/
│   ├── legal/
│   ├── more/
│   ├── payments/
│   ├── reminders/
│   ├── services/
│   ├── settings/
│   ├── subscription/
│   └── today/
└── l10n/
```

## Architecture Style

Current implementation — **feature-first hybrid**.

Features обычно организуют собственные:

```text
presentation
domain
data
```

Cross-cutting infrastructure находится в `core/`.

Thin `app/` shell владеет application-wide composition: router, app shell, bootstrap helpers и lifecycle binding.

## Dependency Direction

Типичная chain:

```text
Screen
  ↓
Provider / Controller
  ↓
Domain use case / repository interface
  ↓
Repository implementation
  ↓
Drift / HTTP / device service
```

Не каждый feature использует все layers.

## Representative Chains

### Today

```text
TodayScreen
  → todayOverviewProvider
  → todayRepositoryProvider
  → TodayRepositoryImpl
  → feature repositories / AppDatabase
```

### Registration

```text
RegisterScreen
  → AuthController.register
  → AuthRepositoryImpl
  → AuthRemoteDataSource
  → SecureTokenStorage
  → LocalDatabaseManager
  → PurchaseService.logIn
```

### Purchase

```text
Paywall
  → AccessController.purchasePackage
  → RevenueCatPurchaseService.purchase
  → AccessController.syncBilling
  → backend AccessStatusView
  → secure snapshot
```

## Canonicality

Документ описывает real implementation organization и не требует от каждого future feature одинаковой folder depth.
