# flutter_secure_storage

> Secure device persistence for account/session credentials and cached access state.

## Verified Version

`flutter_secure_storage 11.0.0`

## Stored State

- `aveli_user_id`
- `aveli_access_token`
- `aveli_refresh_token`
- `aveli_access_snapshot_<sanitizedUserId>`

On iOS the implementation uses `KeychainAccessibility.first_unlock_this_device`.

## Boundary

Tokens and access snapshot are intentionally separate from the SQLite professional workspace.

## Replaceability

**Medium.** A replacement must preserve secure-storage semantics, key migration, logout/delete cleanup and snapshot compatibility.
