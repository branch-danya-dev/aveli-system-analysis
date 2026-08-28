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
| `/appointments/:id` | Appointment details on root navigator. |

The workspace uses a `StatefulShellRoute` with four tabs.

## Query Deep Links

Supported date query:

```text
/?date=YYYY-MM-DD
/calendar?date=YYYY-MM-DD
```

The router updates `selectedScheduleDayProvider`.

## Notification Navigation

Notification payload contains appointment id.

Tap flow:

```text
notification payload
  ↓
pendingReminderAppointmentIdProvider
  ↓
AppShell
  ↓
context.push(/appointments/:id)
```
