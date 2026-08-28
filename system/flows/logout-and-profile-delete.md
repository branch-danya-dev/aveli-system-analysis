# Logout and Profile Deletion

## Why These Flows Must Stay Separate

They both end the current authenticated context, but they have different persistence consequences.

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

This is intentionally destructive client cleanup.

## System-Level Invariant

```text
logout
≠
delete workspace
```

and:

```text
access expiry
≠
delete workspace
```

while:

```text
explicit profile delete
→ may delete local workspace data
```

Canonical client lifecycle:

[`../../frontend/auth/session-lifecycle.md`](../../frontend/auth/session-lifecycle.md)
