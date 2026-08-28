# Aveli Backend — Authentication

> Verified backend behavior for registration, sign-in, refresh, logout, and account self-service.

## Authentication Boundary

Authentication establishes account identity. It does not decide workspace entitlement.

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

Verified high-level flow:

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

The source description confirms these side effects but does **not** explicitly state that the entire registration flow is wrapped in one database transaction.

Registration trial:

```text
type   = trial
source = registration
endsAt = now + TRIAL_DAYS
```

Current default: `TRIAL_DAYS = 30`.

Duplicate registration email: `409 AUTH_EMAIL_ALREADY_EXISTS`.

## Sign-In

`POST /v1/auth/login` normalizes/resolves email, verifies the Argon2id password hash, creates a refresh session, issues access + refresh tokens, updates `users.last_login_at`, and never creates another registration trial.

Public invalid-credential result:

```text
401 AUTH_INVALID_CREDENTIALS
```

A dummy hash is used for missing-user login to reduce timing leakage.

## Access Token

Verified Aveli usage:

```text
format: JWT
claims: { sub: userId, email }
TTL: JWT_ACCESS_TTL
default: 15m
```

At each JWT-authenticated request the backend additionally requires:

```text
users.status = active
```

Canonical JWT technology rationale:

[`../stack/jwt/`](../stack/jwt/)

## Refresh Credential

The refresh token is not JWT.

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

Reuse of a revoked refresh token triggers family invalidation:

```text
reused revoked token
        ↓
revoke all active sessions for that user
```

## Logout

`POST /v1/auth/logout` resolves the session by refresh-token hash and revokes it. The operation is idempotent.

## Logout All

`POST /v1/auth/logout-all` is Bearer-authenticated and revokes all user sessions where `revoked_at IS NULL`.

## Current Account

`GET /v1/auth/me` returns the authenticated user view.

## Account Soft Delete

`DELETE /v1/auth/me` performs a verified database transaction:

1. revoke all sessions;
2. revoke all active access grants;
3. set `users.status = deleted`;
4. rewrite `email_normalized` to `deleted:<userId>:<oldEmail>`.

The rewrite releases the normalized email for future registration.

This operation does not delete the device-local Drift workspace.

### Endpoint Idempotency Note

The supplied source description says account deletion is idempotent for an already deleted account. At the same time, `JwtStrategy` is documented as allowing only `users.status === active`.

Therefore **service-level idempotency vs repeated HTTP delete behavior should be verified directly in controller/guard code before a stronger endpoint guarantee is documented**.

The current API contract does not promise repeated HTTP deletion after the account has already become `deleted`.

## Not Implemented

These routes currently return `501 AUTH_NOT_IMPLEMENTED`:

```text
POST /v1/auth/resend-verification
POST /v1/auth/verify-email
POST /v1/auth/forgot-password
POST /v1/auth/reset-password
```

They are contract stubs, not working product capabilities.

## Related Documentation

- [`session-lifecycle.md`](session-lifecycle.md)
- [`../api/auth/`](../api/auth/)
- [`../stack/jwt/`](../stack/jwt/)
- [`../stack/argon2id/`](../stack/argon2id/)
- [`../../database/server/entities/auth_sessions.md`](../../database/server/entities/auth_sessions.md)
