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

Notification initialization is lazy.

Timezone initialization uses the device timezone with fallback behavior in the client.

## Android

Verified:

```text
channel id: aveli_visit_reminders_sakura
schedule mode: inexactAllowWhileIdle
POST_NOTIFICATIONS permission
ScheduledNotificationBootReceiver
ScheduledNotificationReceiver
```

Exact-alarm permission is not declared.

## iOS

The client requests:

```text
alert
badge
sound
```

notification permissions when reminders are enabled.

## Reminder Contract

Default lead:

```text
1 hour before appointment startsAt
```

Payload:

```text
appointmentId
```

Notification id is derived from appointment id using an FNV-1a-style hash.

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

Android boot receiver registration is VERIFIED.

However, guaranteed reminder restoration across OEM/device reboot without Aveli opening/resuming remains **OPEN**.

The application performs a full database-driven rebuild on app resume.

## No Server Push

Ordinary workspace reminders do not use FCM/APNs.

Frontend implementation:

[`../../frontend/notifications/`](../../frontend/notifications/)
