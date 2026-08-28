# Auth API Contracts

> Canonical HTTP behavior Aveli authentication/account endpoints.

## Endpoints

| Method | Path | Auth | Success | Notes |
|---|---|---|---|---|
| POST | `/v1/auth/register` | none | `201` auth response | validation; 10/min |
| POST | `/v1/auth/login` | none | `200` auth response | invalid credentials; 20/min |
| POST | `/v1/auth/refresh` | refresh token | `200` rotated response | expired/disabled; 30/min |
| POST | `/v1/auth/logout` | refresh token | `{ "ok": true }` | idempotent session revocation |
| POST | `/v1/auth/logout-all` | Bearer | `{ "ok": true }` | 401/403 |
| GET | `/v1/auth/me` | Bearer | `AuthUser` | 401/403 |
| DELETE | `/v1/auth/me` | Bearer | `{ "ok": true }` | soft delete; 5/min |

## Delete Contract Boundary

Guaranteed public contract — один authenticated profile deletion request. Repeated HTTP delete после successful deletion не guaranteed, потому что subsequent Bearer auth уже может не принимать deleted user.

## 501 Contract Stubs

`resend-verification`, `verify-email`, `forgot-password`, `reset-password` возвращают `501 AUTH_NOT_IMPLEMENTED` и не являются current shipped end-to-end capability.

## 429

Rate limits verified, exact dedicated `429` body не established и не invented.

Canonical machine contract: [`../openapi.yaml`](../openapi.yaml)
