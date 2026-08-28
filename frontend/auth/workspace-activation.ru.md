# Authenticated User → Local Workspace

## Mapping

`AuthController._activateLocalWorkspace(userId)`:

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

Server UUID связывает local workspace selection и RevenueCat customer identity.

## Isolation Rule

Поддерживается одна active session/workspace одновременно.

Новый login открывает DB соответствующего user id.

Current UI не имеет multi-account switcher.

## Legacy Database Caveat

`LocalDatabaseManager` поддерживает:

```text
claimLegacyIfPresent: true
```

но current Flutter UI/source нигде это не вызывает.

Поэтому automatic claim legacy `aveli.db` **не** является verified production behavior.
