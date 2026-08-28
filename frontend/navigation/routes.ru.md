# Route Model

## Router

Canonical implementation:

```text
lib/app/router.dart
appRouterProvider
GoRouter
initialLocation = /bootstrap
```

## Verified Routes

| Path | Purpose |
|---|---|
| `/bootstrap` | Cold start. |
| `/welcome` | Public entry. |
| `/register` | Registration. |
| `/sign-in` | Login. |
| `/access-gate` | Blocked / network-required access state. |
| `/paywall` | Purchase/change-plan host. |
| `/` | Today workspace tab. |
| `/calendar` | Calendar tab. |
| `/clients` | Clients tab. |
| `/clients/:id` | Client details. |
| `/more` | More tab. |
| `/more/services` | Services. |
| `/more/unpaid` | Outstanding payments. |
| `/more/finances` | Period finance. |
| `/more/profile` | Profile. |
| `/more/profile/subscription` | Subscription details. |
| `/more/appearance` | Appearance. |
| `/more/about` | About. |
| `/appointments/:id` | Appointment details на root navigator. |

Workspace использует `StatefulShellRoute` с четырьмя tabs.

## Query Deep Links

```text
/?date=YYYY-MM-DD
/calendar?date=YYYY-MM-DD
```

Router обновляет `selectedScheduleDayProvider`.

## Notification Navigation

```text
notification payload
  ↓
pendingReminderAppointmentIdProvider
  ↓
AppShell
  ↓
context.push(/appointments/:id)
```
