# Offline Workspace Operation

## Principle

Aveli разделяет:

```text
workspace availability
from
continuous backend availability
```

Normal professional work использует local persistence.

## Local Operations

При открытом workspace normal operations используют:

```text
SQLite
local files
local reminders
local settings
```

без continuous sync professional data в backend.

## Access Trust

Client может сохранять verified `AccessState` в secure storage.

При unavailable backend/network:

```text
valid trusted snapshot
→ workspace доступен в пределах policy

missing / expired verification
→ needsNetwork
```

Server `nextVerificationRequiredAt` preferred.

Current client также содержит 72-hour default policy fallback.

Это implementation default, не permanent business constant.

## Network Recovery

Recovery:

```text
user retry
app resume
billing sync
access refresh
```

## Critical Separation

```text
Network unavailable
≠
professional workspace erased

Access verification expired
≠
professional workspace erased
```

Canonical:

[`../../frontend/offline/`](../../frontend/offline/)
