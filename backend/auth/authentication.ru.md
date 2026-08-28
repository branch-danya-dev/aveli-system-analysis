# Aveli Backend — Authentication

> Проверенное backend behavior для registration, sign-in, refresh, logout и account self-service.

## Authentication Boundary

Authentication устанавливает account identity и не решает workspace entitlement.

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

Проверенный high-level flow:

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

Source description подтверждает эти side effects, но **не утверждает явно**, что весь registration flow выполняется внутри одной database transaction.

Registration trial:

```text
type   = trial
source = registration
endsAt = now + TRIAL_DAYS
```

Current default: `TRIAL_DAYS = 30`.

Duplicate registration email: `409 AUTH_EMAIL_ALREADY_EXISTS`.

## Sign-In

`POST /v1/auth/login` нормализует/resolves email, проверяет Argon2id password hash, создает refresh session, выдает access + refresh tokens, обновляет `users.last_login_at` и не создает новый registration trial.

Public invalid-credential result:

```text
401 AUTH_INVALID_CREDENTIALS
```

Для missing-user login используется dummy hash, уменьшающий timing leakage.

## Access Token

Verified Aveli usage:

```text
format: JWT
claims: { sub: userId, email }
TTL: JWT_ACCESS_TTL
default: 15m
```

При каждом JWT-authenticated request backend дополнительно требует:

```text
users.status = active
```

Canonical JWT technology rationale:

[`../stack/jwt/`](../stack/jwt/)

## Refresh Credential

Refresh token не JWT.

```text
48 random bytes
→ base64url opaque token
→ SHA-256 hash stored server-side
```

Default refresh-session TTL:

```text
JWT_REFRESH_TTL_DAYS = 60 days
```

## Refresh Rotation

Endpoint: `POST /v1/auth/refresh`

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

Reuse revoked refresh token приводит к family invalidation:

```text
reused revoked token
        ↓
revoke all active sessions for that user
```

## Logout

`POST /v1/auth/logout` находит session по refresh-token hash и revokes ее. Operation идемпотентна.

## Logout All

`POST /v1/auth/logout-all` использует Bearer auth и revokes все user sessions, где `revoked_at IS NULL`.

## Current Account

`GET /v1/auth/me` возвращает authenticated user view.

## Account Soft Delete

`DELETE /v1/auth/me` выполняет verified database transaction:

1. revoke all sessions;
2. revoke all active access grants;
3. set `users.status = deleted`;
4. rewrite `email_normalized` в `deleted:<userId>:<oldEmail>`.

Rewrite освобождает normalized email для будущей registration.

Operation не удаляет device-local Drift workspace.

### Endpoint Idempotency Note

Source description говорит, что account deletion идемпотентен для уже deleted account. Одновременно `JwtStrategy` описан как допускающий только `users.status === active`.

Поэтому **service-level idempotency и repeated HTTP delete behavior нужно сверить по controller/guard code до более сильной endpoint guarantee**.

Current API contract не обещает повторный HTTP delete после перевода account в `deleted`.

## Not Implemented

Эти routes сейчас возвращают `501 AUTH_NOT_IMPLEMENTED`:

```text
POST /v1/auth/resend-verification
POST /v1/auth/verify-email
POST /v1/auth/forgot-password
POST /v1/auth/reset-password
```

Это contract stubs, а не working product capabilities.

## Связанная документация

- [`session-lifecycle.ru.md`](session-lifecycle.ru.md)
- [`../api/auth/`](../api/auth/)
- [`../stack/jwt/`](../stack/jwt/)
- [`../stack/argon2id/`](../stack/argon2id/)
- [`../../database/server/entities/auth_sessions.ru.md`](../../database/server/entities/auth_sessions.ru.md)
