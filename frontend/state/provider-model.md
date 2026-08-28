# Riverpod Provider Model

## Container

Aveli creates `ProviderContainer` manually in `main.dart` and exposes it with `UncontrolledProviderScope`.

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
| `appRouterProvider` | Router and redirect. |
| `appBootstrapControllerProvider` | Cold-start state. |
| `purchaseServiceProvider` | RevenueCat or no-op purchase service. |
| `visitReminderSchedulerProvider` | OS reminder scheduler. |
| `scheduleConfigProvider` | Working schedule. |
| `themeManagerProvider` / `appLocaleProvider` | UI preferences. |

## Lifetime

Most repository providers are long-lived.

A verified `autoDispose` example is:

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

Provider wiring should not redefine business rules; it composes implementation dependencies.
