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

Initialization lazy через `ensureInitialized()`.

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

Notification id derived из appointment id через FNV-style hash.

## Lifecycle

- create/reschedule → schedule/update reminder;
- complete/cancel/delete → cancel reminder;
- app shell startup/resume → rebuild upcoming reminders;
- logout → cancel all reminders.

Startup rebuild использует `VisitReminderService.syncUpcoming`: cancelAll + reschedule upcoming.

## Navigation

Cold-start notification launch details → `pendingReminderAppointmentIdProvider` → `AppShell` → `/appointments/:id`.

## Open Question

Explicit reboot-time rescheduling без следующего app resume current source evidence не подтверждает.
