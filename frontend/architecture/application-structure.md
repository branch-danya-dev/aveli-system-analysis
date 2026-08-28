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

The current implementation is a **feature-first hybrid**.

Features generally organize their own:

```text
presentation
domain
data
```

Cross-cutting infrastructure lives in `core/`.

The thin `app/` shell owns application-wide composition such as router, app shell, bootstrap helpers and lifecycle binding.

## Dependency Direction

A common feature chain is:

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

Not every feature uses every layer.

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

This document describes the real implementation organization.

It does not require every future feature to use identical folder depth.
