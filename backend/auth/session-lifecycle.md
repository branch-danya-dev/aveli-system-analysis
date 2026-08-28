# Aveli Backend — Session Lifecycle

> Verified Aveli lifecycle of JWT access authentication and persisted rotating refresh sessions.

## Ownership

Canonical JWT technology knowledge lives in [`../stack/jwt/`](../stack/jwt/).

This document owns the **Aveli-specific configuration and lifecycle**.

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

`JwtStrategy` additionally verifies `users.status === active`.

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

The plaintext refresh token is not stored in PostgreSQL.

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

Physical ownership: [`../../database/server/entities/auth_sessions.md`](../../database/server/entities/auth_sessions.md).

## Session States

```text
ACTIVE
EXPIRED
REVOKED
```

Rotation is an operation, not a persistent state.

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

Single logout revokes the session matching the supplied refresh-token hash.

`logout-all` revokes all non-revoked sessions for the current user.

## Account Deletion

Soft deletion revokes all account sessions before the user is marked `deleted`.

## Open Details

The current source description does not specify:

- exact JWT signing algorithm;
- device-session count limits;
- server-side denylisting of old access JWTs after refresh/logout.

These details remain intentionally open.

## Related Documentation

- [`authentication.md`](authentication.md)
- [`../api/auth/`](../api/auth/)
- [`../security/`](../security/)
- [`../stack/jwt/`](../stack/jwt/)
