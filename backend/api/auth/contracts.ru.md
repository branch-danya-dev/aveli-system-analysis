# Auth API Contracts

> Канонический HTTP contract для `/v1/auth/*`. JSON fields, paths, identifiers и error codes сохраняются без перевода.

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

Auth: отсутствует  
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
| `email` | valid email; normalize → lowercase + trim |
| `password` | 8–128 characters |
| `deviceId` | optional string |
| `deviceName` | optional string |
| `platform` | optional string |

Errors: `409 AUTH_EMAIL_ALREADY_EXISTS`, `400 AUTH_VALIDATION_FAILED`  
Rate limit: `10/min`

## `POST /v1/auth/login`

Auth: отсутствует  
Success: `200` → `AuthTokensResponse`

Request имеет ту же credential/device-context структуру, что register.

Error: `401 AUTH_INVALID_CREDENTIALS`  
Rate limit: `20/min`

Public credential error намеренно unified.

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

Operation идемпотентна.

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

Backend выполняет soft account deletion и не удаляет local workspace data.

Rate limit: `5/min`

Errors:

```text
401 — invalid/expired authentication
403 AUTH_USER_DISABLED
```

Implementation description говорит о service-level idempotency для already deleted account, но `JwtStrategy` одновременно описан как допускающий только `active` users. Поэтому repeated HTTP deletion после delete не гарантируется этим contract до прямой проверки controller/guard behavior.

## Current 501 Stubs

Все возвращают `501 AUTH_NOT_IMPLEMENTED`.

| Method | Path | Body | Auth |
|---|---|---|---|
| POST | `/v1/auth/resend-verification` | none | Bearer |
| POST | `/v1/auth/verify-email` | `{ "token": "..." }` | none |
| POST | `/v1/auth/forgot-password` | `{ "email": "..." }` | none |
| POST | `/v1/auth/reset-password` | `{ "token": "...", "password": "..." }` | none |

Rate limits: forgot-password `5/min`, reset-password `5/min`.

Это contract stubs, а не реализованные product capabilities.
