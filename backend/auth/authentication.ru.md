# Aveli Backend — Authentication

> Проверенное backend behavior для registration, sign-in, refresh, logout и account self-service.

## Authentication Boundary

Authentication устанавливает account identity.

Она не решает workspace entitlement.

```text
Credentials / Session
        ↓
Authentication
        ↓
Authenticated user
        ↓
Access Resolution
        ↓
Granted / Denied
```

## Registration

Endpoint:

```text
POST /v1/auth/register
```

High-level transaction:

```text
validate DTO
    ↓
normalize email
    ↓
create users row
    ↓
create one registration trial
    ↓
create auth_sessions row
    ↓
issue access + refresh tokens
```

Registration trial:

```text
type   = trial
source = registration
endsAt = now + TRIAL_DAYS
```

Current default:

```text
TRIAL_DAYS = 30
```

Duplicate registration email:

```text
409 AUTH_EMAIL_ALREADY_EXISTS
```

## Sign-In

Endpoint:

```text
POST /v1/auth/login
```

Behavior:

- normalize/resolve email;
- verify Argon2id password hash;
- create authenticated refresh session;
- issue access + refresh tokens;
- update `users.last_login_at`;
- never create another registration trial.

Public invalid-credential result unified:

```text
401 AUTH_INVALID_CREDENTIALS
```

Для missing-user login используется dummy hash, уменьшающий timing leakage.

## Access Token

Access authentication использует JWT.

Verified claims:

```json
{
  "sub": "user UUID",
  "email": "user email"
}
```

Default access-token TTL:

```text
15m
```

При каждом JWT-authenticated request backend user status должен оставаться:

```text
active
```

## Refresh Credential

Refresh token не JWT.

Это:

```text
48 random bytes
→ base64url opaque token
→ SHA-256 hash stored server-side
```

Default refresh-session TTL:

```text
60 days
```

## Refresh Rotation

Endpoint:

```text
POST /v1/auth/refresh
```

Verified behavior:

```text
present refresh token
        ↓
hash + resolve session
        ↓
validate user/session
        ↓
revoke old session
        ↓
create new session
        ↓
issue new access + refresh pair
```

### Reuse Detection

Reuse revoked refresh token:

```text
revoked refresh reuse detected
        ↓
revoke all active sessions for that user
```

Это family invalidation.

## Logout

```text
POST /v1/auth/logout
```

Server находит session по refresh-token hash и revokes ее.

Operation идемпотентна.

Response:

```json
{ "ok": true }
```

## Logout All

```text
POST /v1/auth/logout-all
Authorization: Bearer <accessToken>
```

Revokes все user sessions где:

```text
revoked_at IS NULL
```

## Current Account

```text
GET /v1/auth/me
```

Возвращает authenticated user view.

## Account Soft Delete

```text
DELETE /v1/auth/me
```

Verified transaction:

1. revoke all sessions;
2. revoke all active access grants;
3. set `users.status = deleted`;
4. rewrite `email_normalized` в `deleted:<userId>:<oldEmail>`.

Rewrite освобождает normalized email для будущей registration.

Operation не удаляет device-local Drift workspace.

## Not Implemented

Следующие routes сейчас возвращают:

```text
501 AUTH_NOT_IMPLEMENTED
```

```text
POST /v1/auth/resend-verification
POST /v1/auth/verify-email
POST /v1/auth/forgot-password
POST /v1/auth/reset-password
```

Они должны документироваться как stubs, а не working behavior.

## Canonical Contract

[`../api/auth/`](../api/auth/)

## Связанная документация

- [`session-lifecycle.ru.md`](session-lifecycle.ru.md)
- [`../stack/jwt/`](../stack/jwt/)
- [`../stack/argon2id/`](../stack/argon2id/)
- [`../../database/server/entities/auth_sessions.ru.md`](../../database/server/entities/auth_sessions.ru.md)
