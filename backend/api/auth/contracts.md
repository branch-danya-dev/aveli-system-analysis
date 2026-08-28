# Auth API Contracts

> Canonical HTTP behavior for Aveli authentication/account endpoints.

## Shared Auth Response

```json
{
  "user": { "id": "uuid", "email": "master@example.com", "emailVerified": false },
  "accessToken": "...",
  "refreshToken": "..."
}
```

## Endpoints

| Method | Path | Auth | Success | Notes |
|---|---|---|---|---|
| POST | `/v1/auth/register` | none | `201` auth response | validation; `409 AUTH_EMAIL_ALREADY_EXISTS`; 10/min |
| POST | `/v1/auth/login` | none | `200` auth response | `401 AUTH_INVALID_CREDENTIALS`; 20/min |
| POST | `/v1/auth/refresh` | refresh token | `200` rotated auth response | `401 AUTH_SESSION_EXPIRED`, `403 AUTH_USER_DISABLED`; 30/min |
| POST | `/v1/auth/logout` | refresh token | `{ "ok": true }` | idempotent refresh-session revocation |
| POST | `/v1/auth/logout-all` | Bearer | `{ "ok": true }` | 401 / `403 AUTH_USER_DISABLED` |
| GET | `/v1/auth/me` | Bearer | `AuthUser` | 401 / `403 AUTH_USER_DISABLED` |
| DELETE | `/v1/auth/me` | Bearer | `{ "ok": true }` | soft account deletion; 5/min |

## Delete Contract Boundary

The public contract guarantees one authenticated profile deletion request.

Service internals are described as idempotent for an already-deleted account, while Bearer authentication accepts only an active user. Therefore a second HTTP `DELETE /v1/auth/me` after successful deletion is **outside the guaranteed public contract**. Callers must not rely on repeated post-deletion HTTP idempotency.

The backend does not delete the mobile professional workspace; current frontend profile deletion performs its own explicit local cleanup.

## 501 Contract Stubs

| Method | Path | Auth |
|---|---|---|
| POST | `/v1/auth/resend-verification` | Bearer |
| POST | `/v1/auth/verify-email` | none |
| POST | `/v1/auth/forgot-password` | none |
| POST | `/v1/auth/reset-password` | none |

They return `501 AUTH_NOT_IMPLEMENTED` and are not current shipped end-to-end capabilities.

## Rate-Limit Error Shape

Route limits are implementation-verified, but a dedicated canonical `429` response body is not established by current evidence. No body is invented here.

Canonical machine contract: [`../openapi.yaml`](../openapi.yaml)
