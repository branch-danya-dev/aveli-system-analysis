# flutter_secure_storage

> Secure device persistence account/session credentials и cached access state.

## Verified Version

`flutter_secure_storage 11.0.0`

## Stored State

- `aveli_user_id`
- `aveli_access_token`
- `aveli_refresh_token`
- `aveli_access_snapshot_<sanitizedUserId>`

На iOS используется `KeychainAccessibility.first_unlock_this_device`.

## Boundary

Tokens/access snapshot намеренно отделены от SQLite professional workspace.

## Replaceability

**Medium.** Replacement должен сохранить secure-storage semantics, key migration, logout/delete cleanup и snapshot compatibility.
