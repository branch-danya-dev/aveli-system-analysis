# flutter_local_notifications

> Device-local notification technology for visit reminders.

## Verified Version

`flutter_local_notifications 22.3.0`

## Role

Used by `LocalVisitReminderScheduler` for appointment reminders, payload navigation and startup notification-launch handling.

Timezone handling uses `timezone` + `flutter_timezone`.

## Replaceability

**Medium.** Scheduler abstraction limits impact, but platform configuration, payload behavior and alarm semantics would need migration.
