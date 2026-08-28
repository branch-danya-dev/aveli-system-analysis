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

Flutter mirror:

```text
lib/features/auth/domain/entities/auth_session.dart
```

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

Errors:

```text
409 AUTH_EMAIL_ALREADY_EXISTS
400 AUTH_VALIDATION_FAILED
```

Rate limit:

```text
10/min
```

## `POST /v1/auth/login`

Auth: none  
Success: `200` → `AuthTokensResponse`

Request shape matches register credentials/device context.

Errors:

```text
401 AUTH_INVALID_CREDENTIALS
```

Rate limit:

```text
20/min
```

The public credential error is intentionally unified.

## `POST /v1/auth/refresh`

Auth: refresh credential in body  
Success: `200` → `AuthTokensResponse`

Request:

```json
{
  "refreshToken": "..."
}
```

Errors:

```text
401 AUTH_SESSION_EXPIRED
403 AUTH_USER_DISABLED
```

Rate limit:

```text
30/min
```

## `POST /v1/auth/logout`

Request:

```json
{
  "refreshToken": "..."
}
```

Success:

```json
{
  "ok": true
}
```

The operation is idempotent.

## `POST /v1/auth/logout-all`

Auth:

```http
Authorization: Bearer <accessToken>
```

Success:

```json
{
  "ok": true
}
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

## `DELETE /v1/auth/me`

Auth: Bearer

Success:

```json
{
  "ok": true
}
```

The backend performs soft account deletion. The API contract does not imply deletion of local workspace data.

Rate limit:

```text
5/min
```

## Current 501 Stubs

All return:

```text
501 AUTH_NOT_IMPLEMENTED
```

| Method | Path | Body | Auth |
|---|---|---|---|
| POST | `/v1/auth/resend-verification` | none | Bearer |
| POST | `/v1/auth/verify-email` | `{ "token": "..." }` | none |
| POST | `/v1/auth/forgot-password` | `{ "email": "..." }` | none |
| POST | `/v1/auth/reset-password` | `{ "token": "...", "password": "..." }` | none |

Rate limits:

```text
forgot-password: 5/min
reset-password:  5/min
```

These routes exist as contract stubs, not implemented product capabilities.


> RU-версия сохраняет JSON, paths, identifiers и error codes без перевода. Описательные правила соответствуют English contract выше.
