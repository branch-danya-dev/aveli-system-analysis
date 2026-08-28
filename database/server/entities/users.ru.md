# `users`

> Backend records пользовательской identity.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Идентификатор пользователя. |
| `email` | TEXT | Email в исходном пользовательском представлении. |
| `email_normalized` | TEXT UNIQUE | Нормализованный email для sign-in: lowercase / trimmed. |
| `password_hash` | TEXT | Argon2id hash пароля; plaintext не хранится. |
| `email_verified_at` | TIMESTAMPTZ nullable | Время подтверждения email. |
| `status` | UserStatus | Lifecycle state: `active`, `disabled`, `deleted`. |
| `created_at` | TIMESTAMPTZ | Время создания. |
| `updated_at` | TIMESTAMPTZ | Время последнего обновления. |
| `last_login_at` | TIMESTAMPTZ nullable | Время последнего успешного login. |

## Related Documentation

- [`../schema/enums.ru.md`](../schema/enums.ru.md)
