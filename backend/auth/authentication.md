# Aveli Backend — Authentication

> Verified backend behavior for registration, sign-in, refresh, logout, and account self-service.

## Authentication Boundary

Authentication establishes account identity.

It does not decide workspace entitlement.

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

Public invalid-credential result is unified:

```text
401 AUTH_INVALID_CREDENTIALS
```

A dummy hash is used for missing-user login to reduce timing leakage.

## Access Token

Access authentication uses JWT.

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

At each JWT-authenticated request, backend user status must still be:

```text
active
```

## Refresh Credential

The refresh token is not JWT.

It is:

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

If a revoked refresh token is reused:

```text
revoked refresh reuse detected
        ↓
revoke all active sessions for that user
```

This is family invalidation.

## Logout

```text
POST /v1/auth/logout
```

The server resolves the session by refresh-token hash and revokes it.

The operation is idempotent.

Response:

```json
{ "ok": true }
```

## Logout All

```text
POST /v1/auth/logout-all
Authorization: Bearer <accessToken>
```

Revokes all user sessions where:

```text
revoked_at IS NULL
```

## Current Account

```text
GET /v1/auth/me
```

Returns the authenticated user view.

## Account Soft Delete

```text
DELETE /v1/auth/me
```

Verified transaction:

1. revoke all sessions;
2. revoke all active access grants;
3. set `users.status = deleted`;
4. rewrite `email_normalized` to `deleted:<userId>:<oldEmail>`.

The rewrite releases the normalized email for future registration.

This operation does not delete the user's device-local Drift workspace.

## Not Implemented

The following routes currently return:

```text
501 AUTH_NOT_IMPLEMENTED
```

Routes:

```text
POST /v1/auth/resend-verification
POST /v1/auth/verify-email
POST /v1/auth/forgot-password
POST /v1/auth/reset-password
```

They must be documented as stubs rather than planned/working behavior.

## Canonical Contract

[`../api/auth/`](../api/auth/)

## Related Documentation

- [`session-lifecycle.md`](session-lifecycle.md)
- [`../stack/jwt/`](../stack/jwt/)
- [`../stack/argon2id/`](../stack/argon2id/)
- [`../../database/server/entities/auth_sessions.md`](../../database/server/entities/auth_sessions.md)
