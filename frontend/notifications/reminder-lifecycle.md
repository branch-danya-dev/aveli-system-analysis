# Visit Reminder Lifecycle

## Technology

```text
flutter_local_notifications
timezone
flutter_timezone
```

Scheduler:

```text
LocalVisitReminderScheduler
```

Initialization is lazy through `ensureInitialized()`.

## Configuration

Android channel:

```text
aveli_visit_reminders_sakura
```

Default lead:

```text
1 hour before startsAt
```

Payload:

```text
appointmentId
```

Notification id is derived from appointment id using an FNV-style hash.

## Lifecycle

- create/reschedule appointment → schedule/update reminder;
- complete/cancel/delete → cancel reminder;
- app shell startup/resume → rebuild upcoming reminders;
- logout → cancel all reminders.

Startup rebuild uses `VisitReminderService.syncUpcoming`, which cancels all and reschedules upcoming items.

## Navigation

Cold-start notification launch details are translated into `pendingReminderAppointmentIdProvider`, then `AppShell` navigates to `/appointments/:id`.

## Open Question

Explicit reboot-time rescheduling independent from the next app resume is not established by the current source evidence.
