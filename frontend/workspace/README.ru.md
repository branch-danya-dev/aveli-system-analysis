# Professional Workspace

> Frontend feature map device-owned operational workspace.

## Ownership

Workspace entities остаются device-owned и сохраняются через Drift/SQLite и local files.

Canonical schema:

[`../../database/local/`](../../database/local/)

## Feature Map

| Feature | Main client responsibility |
|---|---|
| Clients | CRM records, archive/delete/import behavior. |
| Services | Service catalog editing. |
| Appointments | Booking, reschedule, completion/cancellation, visit data. |
| Today / Calendar | Reactive schedule projections и navigation. |
| Payments | Outstanding payments и finance views. |
| Visit Notes / Photos | Visit documentation и local media. |
| Settings / Profile | Local profile/preferences, import/export, currency. |
| Schedule | Working-hours model и slot validation. |

## Навигация

- [`feature-map.ru.md`](feature-map.ru.md)
- [`device-integrations.ru.md`](device-integrations.ru.md)
