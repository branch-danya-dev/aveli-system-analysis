# JWT

> Access-token format для authenticated Aveli backend requests.

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

При authenticated request `JwtStrategy` дополнительно проверяет backend user:

```text
status = active
```

Поэтому structurally valid token не обходит current account-state validation.

## Refresh Token Is Not JWT

Refresh credentials:

```text
opaque
48 random bytes
base64url encoded
```

Server persistence хранит только:

```text
SHA-256 hash
```

Refresh-session TTL:

```text
JWT_REFRESH_TTL_DAYS
default: 60 days
```

Важно:

```text
JWT access token
        ≠
opaque refresh token
```

## Contextual Usage

Authentication lifecycle:

[`../../auth/`](../../auth/)

Physical refresh-session state:

[`../../../database/server/entities/auth_sessions.ru.md`](../../../database/server/entities/auth_sessions.ru.md)
