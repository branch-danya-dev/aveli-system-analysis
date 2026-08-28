# Aveli Backend — Session Lifecycle

> Проверенный lifecycle JWT access authentication и persisted rotating refresh sessions.

## Credential Pair

Aveli authentication выдает:

```text
JWT Access Token
        +
Opaque Refresh Token
```

У них разные lifecycle и trust properties.

## Access Token

Verified defaults:

```text
claims: { sub: userId, email }
TTL: JWT_ACCESS_TTL
default: 15m
```

`JwtStrategy` дополнительно проверяет:

```text
users.status === active
```

## Refresh Token

Verified format:

```text
48 random bytes
base64url opaque string
```

Stored value:

```text
SHA-256(refreshToken)
```

Plaintext refresh token не хранится в PostgreSQL.

Refresh-session TTL:

```text
JWT_REFRESH_TTL_DAYS
default: 60 days
```

## Persisted Session State

Canonical physical fields:

```text
id
user_id
refresh_token_hash
device_id
device_name
platform
created_at
last_used_at
expires_at
revoked_at
```

## Session States

```text
ACTIVE
EXPIRED
REVOKED
```

## Rotation

Successful refresh выполняет rotation:

```text
Old active session
      ↓
Validate
      ↓
Revoke old session
      ↓
Create new session
      ↓
Issue new token pair
```

Old refresh token становится invalid.

## Reuse / Family Invalidation

Reuse revoked refresh token считается high-risk condition.

Verified response:

```text
reused revoked token
      ↓
revoke all active sessions of the user
      ↓
require re-authentication
```

## Logout

Single logout revokes session, соответствующую refresh-token hash.

Endpoint идемпотентен.

## Logout All

Authenticated `logout-all` revokes все non-revoked sessions current user.

## Account Deletion

Soft deletion сначала revokes все sessions, затем переводит user в `deleted`.

## Open Detail

Current source description не указывает:

- exact JWT signing algorithm;
- Argon2 parameters;
- device session count limits;
- server-side denylist старых access JWT после refresh/logout.

Эти детали остаются open.

## Связанная документация

- [`authentication.ru.md`](authentication.ru.md)
- [`../api/auth/`](../api/auth/)
- [`../security/`](../security/)
- [`../../database/server/entities/auth_sessions.ru.md`](../../database/server/entities/auth_sessions.ru.md)
