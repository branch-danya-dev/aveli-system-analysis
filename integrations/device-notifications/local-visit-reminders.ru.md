# Local Visit Reminder Boundary

## Technology

```text
flutter_local_notifications 22.3.0
timezone
flutter_timezone
```

Scheduler:

```text
LocalVisitReminderScheduler
```

## Initialization

Notification initialization lazy.

Timezone initialization использует device timezone с client fallback behavior.

## Android

Verified:

```text
channel id: aveli_visit_reminders_sakura
schedule mode: inexactAllowWhileIdle
POST_NOTIFICATIONS permission
ScheduledNotificationBootReceiver
ScheduledNotificationReceiver
```

Exact-alarm permission не declared.

## iOS

Client запрашивает:

```text
alert
badge
sound
```

permissions при enable reminders.

## Reminder Contract

Default lead:

```text
1 hour before appointment startsAt
```

Payload:

```text
appointmentId
```

Notification id derived из appointment id через FNV-1a-style hash.

## Lifecycle

```text
create/reschedule
→ schedule reminder

complete/cancel/delete
→ cancel reminder

logout
→ cancel all

app start/resume
→ rebuild upcoming reminders
```

## Tap Flow

```text
OS notification tap
  ↓
appointmentId payload
  ↓
pendingReminderAppointmentIdProvider
  ↓
/appointments/:id
```

## Reboot Behavior

Android boot receiver registration VERIFIED.

Но guaranteed reminder restoration после OEM/device reboot без открытия/resume Aveli остается **OPEN**.

App выполняет full DB-driven rebuild на resume.

## No Server Push

Ordinary workspace reminders не используют FCM/APNs.

Frontend implementation:

[`../../frontend/notifications/`](../../frontend/notifications/)
