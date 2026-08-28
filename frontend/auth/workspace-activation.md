# Authenticated User → Local Workspace

## Mapping

`AuthController._activateLocalWorkspace(userId)` performs:

```text
server users.id
   ↓
LocalDatabaseManager.openForUser(userId)
   ↓
aveli_<userId>.sqlite

userId
   ↓
PurchaseService.logIn(userId)
   ↓
RevenueCat App User ID
```

The server UUID is therefore the linking identity for both local workspace selection and RevenueCat customer identity.

## Isolation Rule

Only one active session/workspace is supported at a time.

A new login opens the database matching that user id.

The current UI does not provide a multi-account switcher.

## Legacy Database Caveat

`LocalDatabaseManager` exposes legacy-claim support:

```text
claimLegacyIfPresent: true
```

but no current Flutter UI/source call uses it.

Therefore automatic claim of a legacy `aveli.db` is **not** part of verified production client behavior.
