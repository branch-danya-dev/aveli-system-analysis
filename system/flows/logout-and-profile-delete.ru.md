# Logout and Profile Deletion

## Why These Flows Must Stay Separate

Оба flow завершают current authenticated context, но persistence consequences разные.

## Logout

```text
User logout
    ↓
cancel local visit reminders
    ↓
clear current access snapshot
    ↓
RevenueCat logOut
    ↓
revoke backend refresh session / clear credentials
    ↓
close current local DB
    ↓
return to auth flow
```

Preserved:

```text
SQLite workspace file
visit photos
professional history
```

## Profile Delete

```text
DELETE /v1/auth/me
    ↓
backend account soft-delete
    ↓
revoke sessions + grants
    ↓
client deletes local user photo tree
    ↓
client deletes local SQLite data
    ↓
clear snapshot/session state
```

Это intentionally destructive client cleanup.

## System-Level Invariant

```text
logout
≠
delete workspace
```

и:

```text
access expiry
≠
delete workspace
```

но:

```text
explicit profile delete
→ may delete local workspace data
```

Canonical:

[`../../frontend/auth/session-lifecycle.ru.md`](../../frontend/auth/session-lifecycle.ru.md)
