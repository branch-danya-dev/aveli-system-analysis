# JWT

> Access-token format for authenticated Aveli backend requests.

## Verified Access JWT

Payload:

```json
{
  "sub": "<userId>",
  "email": "<account email>"
}
```

Configuration:

```text
secret: JWT_ACCESS_SECRET
TTL:    JWT_ACCESS_TTL
default TTL: 15m
```

At authenticated request time, `JwtStrategy` also verifies that the corresponding backend user has:

```text
status = active
```

A structurally valid token therefore does not bypass current account-state validation.

## Refresh Token Is Not JWT

Refresh credentials are:

```text
opaque
48 random bytes
base64url encoded
```

Server persistence stores only a:

```text
SHA-256 hash
```

Refresh-session TTL:

```text
JWT_REFRESH_TTL_DAYS
default: 60 days
```

This distinction is important:

```text
JWT access token
        ≠
opaque refresh token
```

## Contextual Usage

Authentication lifecycle:

[`../../auth/`](../../auth/)

Physical refresh-session state:

[`../../../database/server/entities/auth_sessions.md`](../../../database/server/entities/auth_sessions.md)
