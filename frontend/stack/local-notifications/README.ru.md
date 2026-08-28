# flutter_local_notifications

> Device-local notification technology visit reminders.

## Verified Version

`flutter_local_notifications 22.3.0`

## Role

`LocalVisitReminderScheduler` использует package для appointment reminders, payload navigation и startup notification-launch handling.

Timezone handling: `timezone` + `flutter_timezone`.

## Replaceability

**Medium.** Scheduler abstraction ограничивает impact, но platform configuration, payload behavior и alarm semantics потребуют migration.
