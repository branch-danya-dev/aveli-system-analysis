# Riverpod Provider Model

## Container

Aveli вручную создает `ProviderContainer` в `main.dart` и exposes его через `UncontrolledProviderScope`.

## Cross-Cutting Providers

| Provider | Role |
|---|---|
| `localDatabaseManagerProvider` | Per-user DB open/close. |
| `appDatabaseProvider` | Active `AppDatabase`. |
| `authControllerProvider` | Session + workspace activation. |
| `accessControllerProvider` | Server access + snapshot. |
| `accessVerificationPolicyProvider` | Offline trust policy. |
| `accessSnapshotStoreProvider` | Secure snapshot persistence. |
| `networkAvailableProvider` | Connectivity stream. |
| `appRouterProvider` | Router и redirect. |
| `appBootstrapControllerProvider` | Cold-start state. |
| `purchaseServiceProvider` | RevenueCat или no-op purchase service. |
| `visitReminderSchedulerProvider` | OS reminder scheduler. |
| `scheduleConfigProvider` | Working schedule. |
| `themeManagerProvider` / `appLocaleProvider` | UI preferences. |

## Lifetime

Большинство repository providers long-lived.

Verified `autoDispose` example:

```text
paywallOfferingsProvider
```

## Dependency Direction

```text
Presentation
    ↓ watch/read
Provider / Controller
    ↓
Repository / Use Case
    ↓
Infrastructure
```

Provider wiring не должен переопределять business rules; он composes implementation dependencies.
