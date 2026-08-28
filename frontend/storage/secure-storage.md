# Secure Storage Usage

## Keys

| Key | Content |
|---|---|
| `aveli_user_id` | Server user UUID. |
| `aveli_access_token` | JWT access token. |
| `aveli_refresh_token` | Opaque refresh token. |
| `aveli_access_snapshot_<sanitizedUserId>` | JSON `AccessState`. |

Snapshot key sanitization replaces `/` and `\` with `_`.

## iOS

Verified accessibility:

```text
KeychainAccessibility.first_unlock_this_device
```

## Cleanup

Logout clears session credentials and the current user's access snapshot.

Delete profile also removes secure state as part of destructive profile cleanup.

## Boundary

Secure session/access state is deliberately separate from the professional SQLite workspace.
