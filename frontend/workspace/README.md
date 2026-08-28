# Professional Workspace

> Frontend feature map for the device-owned operational workspace.

## Ownership

Workspace entities remain device-owned and are persisted through Drift/SQLite and local files.

Canonical schema:

[`../../database/local/`](../../database/local/)

## Feature Map

| Feature | Main client responsibility |
|---|---|
| Clients | CRM records, archive/delete/import behavior. |
| Services | Service catalog editing. |
| Appointments | Booking, reschedule, completion/cancellation, visit data. |
| Today / Calendar | Reactive schedule projections and navigation. |
| Payments | Outstanding payments and finance views. |
| Visit Notes / Photos | Visit documentation and local media. |
| Settings / Profile | Local profile/preferences, import/export, currency. |
| Schedule | Working-hours model and slot validation. |

## Navigation

- [`feature-map.md`](feature-map.md)
- [`device-integrations.md`](device-integrations.md)
