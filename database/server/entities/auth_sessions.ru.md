# `auth_sessions`

> Persisted refresh-session records.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Session identifier. |
| `user_id` | UUID FK | → users; ON DELETE CASCADE. |
| `refresh_token_hash` | TEXT | Refresh token hash; not plaintext. |
| `device_id` | TEXT nullable | Device identifier. |
| `device_name` | TEXT nullable | Device display name. |
| `platform` | TEXT nullable | android / ios / other. |
| `created_at` | TIMESTAMPTZ | Creation. |
| `last_used_at` | TIMESTAMPTZ | Last use. |
| `expires_at` | TIMESTAMPTZ | Expiry. |
| `revoked_at` | TIMESTAMPTZ nullable | Logout / rotation revocation. |

## Constraints / Indexes

- CHECK: `expires_at > created_at`
- index: `user_id`
- index: `expires_at`
