# Data Ownership and Movement

## Ownership Matrix

| Information | Canonical Owner | Main Persistence |
|---|---|---|
| Clients | Professional workspace | SQLite |
| Services | Professional workspace | SQLite |
| Appointments | Professional workspace | SQLite |
| Payments | Professional workspace | SQLite |
| Visit notes | Professional workspace | SQLite |
| Visit photos | Professional workspace | Local files + SQLite metadata |
| Workspace settings | Professional workspace/client | SQLite |
| Account identity | Backend | PostgreSQL |
| Refresh sessions | Backend | PostgreSQL |
| Trial / grants | Backend | PostgreSQL |
| Subscription snapshot | Backend | PostgreSQL |
| Subscription webhook events | Backend | PostgreSQL |
| Client access snapshot | Frontend cached trust | Secure storage |
| Access/refresh credentials | Frontend client state | Secure storage |

Canonical:

[`../../database/architecture/data-ownership.ru.md`](../../database/architecture/data-ownership.ru.md)

## Data That Crosses Boundaries

### Account / Access

```text
Backend
→ auth tokens
→ Flutter secure storage

Backend
→ AccessStatusView
→ Flutter secure snapshot
```

### RevenueCat

```text
Aveli users.id
→ RevenueCat App User ID

RevenueCat subscription evidence
→ backend subscriptions

RevenueCat webhook
→ subscription_events
```

### Device Contact Import

```text
Device contact
→ name / phone / contact id
→ new/enriched local Client
```

Imported Aveli client дальше owned professional workspace.

### Visit Photos

```text
Camera / Gallery source
→ copied file
→ Aveli local visit-photo tree
→ Drift metadata
```

## Deliberate Non-Movement

Current architecture намеренно **не** перемещает:

```text
clients
appointments
payments
visit notes
visit photos
```

в Aveli backend как synchronized workspace data.

Это important negative boundary.

## Identity Does Not Merge Ownership

Один `users.id` может использоваться для:

```text
local workspace filename
secure snapshot
RevenueCat customer
backend account
```

но это не превращает данные в один persistence aggregate.
