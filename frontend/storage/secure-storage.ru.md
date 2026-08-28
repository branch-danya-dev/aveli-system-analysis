# Secure Storage Usage

## Keys

| Key | Content |
|---|---|
| `aveli_user_id` | Server user UUID. |
| `aveli_access_token` | JWT access token. |
| `aveli_refresh_token` | Opaque refresh token. |
| `aveli_access_snapshot_<sanitizedUserId>` | JSON `AccessState`. |

Snapshot key sanitization заменяет `/` и `\` на `_`.

## iOS

Verified accessibility:

```text
KeychainAccessibility.first_unlock_this_device
```

## Cleanup

Logout очищает session credentials и access snapshot current user.

Delete profile также удаляет secure state как часть destructive cleanup.

## Boundary

Secure session/access state намеренно отделен от professional SQLite workspace.
