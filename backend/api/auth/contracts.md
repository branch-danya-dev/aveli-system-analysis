# Auth API Contracts

## Shared Response Types

### `AuthUser`

```json
{
  "id": "uuid",
  "email": "master@example.com",
  "emailVerified": false
}
```

### `AuthTokensResponse`

```json
{
  "user": {
    "id": "uuid",
    "email": "master@example.com",
    "emailVerified": false
  },
  "accessToken": "...",
  "refreshToken": "..."
}
```

Flutter mirror: `lib/features/auth/domain/entities/auth_session.dart`.

## `POST /v1/auth/register`

Auth: none  
Success: `201 Created` → `AuthTokensResponse`

Request:

```json
{
  "email": "master@example.com",
  "password": "minimum8chars",
  "deviceId": "optional",
  "deviceName": "optional",
  "platform": "optional"
}
```

Validation:

| Field | Rule |
|---|---|
| `email` | valid email; normalized to lowercase + trim |
| `password` | 8–128 characters |
| `deviceId` | optional string |
| `deviceName` | optional string |
| `platform` | optional string |

Errors: `409 AUTH_EMAIL_ALREADY_EXISTS`, `400 AUTH_VALIDATION_FAILED`  
Rate limit: `10/min`

## `POST /v1/auth/login`

Auth: none  
Success: `200` → `AuthTokensResponse`

Request shape matches register credentials/device context.

Error: `401 AUTH_INVALID_CREDENTIALS`  
Rate limit: `20/min`

The public credential error is intentionally unified.

## `POST /v1/auth/refresh`

Request:

```json
{ "refreshToken": "..." }
```

Success: `200` → `AuthTokensResponse`

Errors:

```text
401 AUTH_SESSION_EXPIRED
403 AUTH_USER_DISABLED
```

Rate limit: `30/min`

## `POST /v1/auth/logout`

Request:

```json
{ "refreshToken": "..." }
```

Success:

```json
{ "ok": true }
```

The operation is idempotent.

## `POST /v1/auth/logout-all`

Auth: Bearer

Success:

```json
{ "ok": true }
```

Errors:

```text
401 — invalid/expired authentication
403 AUTH_USER_DISABLED
```

## `GET /v1/auth/me`

Auth: Bearer

Success:

```json
{
  "id": "uuid",
  "email": "master@example.com",
  "emailVerified": false
}
```

Errors:

```text
401 — invalid/expired authentication
403 AUTH_USER_DISABLED
```

## `DELETE /v1/auth/me`

Auth: Bearer

Success:

```json
{ "ok": true }
```

The backend performs soft account deletion and does not delete local workspace data.

Rate limit: `5/min`

Errors:

```text
401 — invalid/expired authentication
403 AUTH_USER_DISABLED
```

The implementation description states service-level idempotency for an already deleted account while `JwtStrategy` is also documented as accepting only `active` users. Repeated HTTP deletion after deletion is therefore not guaranteed by this contract until controller/guard behavior is verified.

## Current 501 Stubs

All return `501 AUTH_NOT_IMPLEMENTED`.

| Method | Path | Body | Auth |
|---|---|---|---|
| POST | `/v1/auth/resend-verification` | none | Bearer |
| POST | `/v1/auth/verify-email` | `{ "token": "..." }` | none |
| POST | `/v1/auth/forgot-password` | `{ "email": "..." }` | none |
| POST | `/v1/auth/reset-password` | `{ "token": "...", "password": "..." }` | none |

Rate limits: forgot-password `5/min`, reset-password `5/min`.

These routes are contract stubs, not implemented product capabilities.
