# Aveli Startup Flow

## Pre-UI Bootstrap

Verified order:

```text
main()
 ↓
WidgetsFlutterBinding.ensureInitialized()
 ↓
ReleaseConfigGate.assertForCurrentBuild()
 ↓
edge-to-edge system UI
 ↓
initialize ru/en date formatting
 ↓
ProviderContainer()
 ↓
hydrate guest locale
 ↓
runApp(UncontrolledProviderScope → AveliApp)
```

Drift здесь **не** открывается.

RevenueCat configuration и notifications initialization lazy.

## Initial Route

`GoRouter.initialLocation`:

```text
/bootstrap
```

Далее `AppBootstrapController` resolves application state.

## Signed-In Initialization

```text
restore session via refresh token
        ↓
open aveli_<userId>.sqlite
        ↓
RevenueCat logIn(userId)
        ↓
restore theme from Drift
        ↓
standalone mode?
   /               \
 yes                no
  ↓                  ↓
ready        refresh /v1/access
                     ↓
              optional debug seed
                     ↓
               wait access state
                     ↓
            resolve Access Gate
```

## Bootstrap Outcomes

- `BootstrapNeedsWelcome`
- `BootstrapReady`
- `BootstrapAccessRequired`
- `BootstrapNeedsVerification`
- `BootstrapFailure`

Bootstrap screen переводит result в router navigation.

## Lazy Services

- `Purchases.configure` → lazy в `RevenueCatPurchaseService.ensureConfigured()`
- notifications init → lazy в `LocalVisitReminderScheduler.ensureInitialized()`
- database → открывается при authenticated workspace activation, не pre-runApp

## Verification Note

Maximum initialization timeout задается `BootstrapTiming.maxInitialization`.
