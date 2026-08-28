# Aveli Backend — Session Lifecycle

> Проверенный Aveli lifecycle JWT access authentication и persisted rotating refresh sessions.

## Ownership

Canonical JWT technology knowledge находится в [`../stack/jwt/`](../stack/jwt/).

Этот документ владеет **Aveli-specific configuration и lifecycle**.

## Credential Pair

```text
JWT Access Token
        +
Opaque Refresh Token
```

## Access Token

Verified configuration:

```text
claims: { sub: userId, email }
TTL: JWT_ACCESS_TTL
default: 15m
```

`JwtStrategy` дополнительно проверяет `users.status === active`.

## Refresh Token

Verified format:

```text
48 random bytes
base64url opaque string
```

Persisted server value:

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

Physical ownership: [`../../database/server/entities/auth_sessions.ru.md`](../../database/server/entities/auth_sessions.ru.md).

## Session States

```text
ACTIVE
EXPIRED
REVOKED
```

Rotation — operation, а не persistent state.

## Rotation

```text
Old active session
      ↓
Validate presented refresh credential
      ↓
Revoke old session
      ↓
Create new session
      ↓
Issue new token pair
```

## Reuse / Family Invalidation

```text
reused revoked token
      ↓
revoke all active sessions of the user
      ↓
require re-authentication
```

## Logout / Logout All

Single logout revokes session по supplied refresh-token hash.

`logout-all` revokes все non-revoked sessions current user.

## Account Deletion

Soft deletion revokes все account sessions до перевода user в `deleted`.

## Open Details

Current source description не указывает:

- exact JWT signing algorithm;
- device-session count limits;
- server-side denylisting старых access JWT после refresh/logout.

Эти детали намеренно остаются open.

## Связанная документация

- [`authentication.ru.md`](authentication.ru.md)
- [`../api/auth/`](../api/auth/)
- [`../security/`](../security/)
- [`../stack/jwt/`](../stack/jwt/)
