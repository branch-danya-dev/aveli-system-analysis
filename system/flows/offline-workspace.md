# Offline Workspace Operation

## Principle

Aveli separates:

```text
workspace availability
from
continuous backend availability
```

Normal professional work uses local persistence.

## Local Operations

While the workspace is open, normal operations can use:

```text
SQLite
local files
local reminders
local settings
```

without continuously synchronizing professional data to the backend.

## Access Trust

The client may persist a verified `AccessState` in secure storage.

When backend/network access is unavailable:

```text
valid trusted snapshot
→ workspace may remain available within policy

missing / expired verification
→ needsNetwork
```

The server-provided `nextVerificationRequiredAt` is preferred.

The current client also contains a 72-hour default policy fallback.

This is an implementation default, not a permanent business constant.

## Network Recovery

Recovery is driven by:

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

Canonical frontend offline model:

[`../../frontend/offline/`](../../frontend/offline/)
