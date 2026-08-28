# Aveli Startup Flow

## Pre-UI Bootstrap

Verified order before/around `runApp`:

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

Drift is **not** opened here.

RevenueCat configuration and notifications initialization are lazy.

## Initial Route

`GoRouter.initialLocation`:

```text
/bootstrap
```

`AppBootstrapController` then resolves application state.

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

The bootstrap screen translates the result into router navigation.

## Lazy Services

- `Purchases.configure` → lazy inside `RevenueCatPurchaseService.ensureConfigured()`
- notifications initialization → lazy inside `LocalVisitReminderScheduler.ensureInitialized()`
- database → opened during authenticated workspace activation, not pre-runApp

## Verification Note

The source exposes a maximum initialization timeout via `BootstrapTiming.maxInitialization`.
