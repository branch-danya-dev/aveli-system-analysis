# Aveli Backend — Session Lifecycle

> Verified lifecycle of JWT access authentication and persisted rotating refresh sessions.

## Credential Pair

Aveli authentication issues:

```text
JWT Access Token
        +
Opaque Refresh Token
```

They have different lifecycles and trust properties.

## Access Token

Verified defaults:

```text
claims: { sub: userId, email }
TTL: JWT_ACCESS_TTL
default: 15m
```

`JwtStrategy` additionally verifies:

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

The plaintext refresh token is not persisted in PostgreSQL.

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

Successful refresh performs rotation:

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

The old refresh token becomes invalid.

## Reuse / Family Invalidation

Reuse of a revoked refresh token is treated as a high-risk condition.

Verified response:

```text
reused revoked token
      ↓
revoke all active sessions of the user
      ↓
require re-authentication
```

## Logout

Single logout revokes the session matching the provided refresh-token hash.

The endpoint is idempotent.

## Logout All

Authenticated `logout-all` revokes all non-revoked sessions for the current user.

## Account Deletion

Soft deletion revokes all account sessions before marking the user `deleted`.

## Open Detail

The current source description does not specify:

- exact JWT signing algorithm;
- Argon2 parameters;
- whether device session counts are limited;
- whether old access JWTs are server-side denylisted after refresh/logout.

Those details remain open.

## Related Documentation

- [`authentication.md`](authentication.md)
- [`../api/auth/`](../api/auth/)
- [`../security/`](../security/)
- [`../../database/server/entities/auth_sessions.md`](../../database/server/entities/auth_sessions.md)
