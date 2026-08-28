# `users`

> Backend user identity records.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | User identifier. |
| `email` | TEXT | Email as entered. |
| `email_normalized` | TEXT UNIQUE | Lowercase/trimmed login identity. |
| `password_hash` | TEXT | Argon2id password hash. |
| `email_verified_at` | TIMESTAMPTZ nullable | Verification timestamp. |
| `status` | UserStatus | active / disabled / deleted. |
| `created_at` | TIMESTAMPTZ | Creation. |
| `updated_at` | TIMESTAMPTZ | Last update. |
| `last_login_at` | TIMESTAMPTZ nullable | Last login. |
