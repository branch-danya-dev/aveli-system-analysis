# `auth_sessions`

> Persisted records refresh sessions.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Идентификатор session. |
| `user_id` | UUID FK | Owning user; → `users`, ON DELETE CASCADE. |
| `refresh_token_hash` | TEXT | Hash refresh token; plaintext token не хранится. |
| `device_id` | TEXT nullable | Идентификатор устройства, если известен. |
| `device_name` | TEXT nullable | Отображаемое имя устройства. |
| `platform` | TEXT nullable | Platform: android / ios / other. |
| `created_at` | TIMESTAMPTZ | Время создания session. |
| `last_used_at` | TIMESTAMPTZ | Последнее использование session. |
| `expires_at` | TIMESTAMPTZ | Граница срока действия. |
| `revoked_at` | TIMESTAMPTZ nullable | Время revoke при logout / rotation. |

## Constraints / Indexes

- CHECK: `expires_at > created_at`
- index: `user_id`
- index: `expires_at`

## Связанная документация

- [`../constraints/invariants.ru.md`](../constraints/invariants.ru.md)
