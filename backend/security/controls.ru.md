# Backend Security Controls

## Implemented Controls

| Concern | Verified Implementation |
|---|---|
| Password storage | Argon2id hash. |
| Refresh-token storage | Только SHA-256 hash. |
| Refresh rotation | Old session revokes on use. |
| Refresh reuse | Reuse revoked token revokes все active user sessions. |
| Timing leakage | Dummy hash для missing-user failed login. |
| Access JWT | JWT bearer token + account-status validation. |
| HTTP headers | `helmet`. |
| Body validation | Whitelist + reject unknown fields. |
| Rate limiting | Global + auth-route throttles. |
| Webhook auth | Exact shared-secret Authorization header. |
| RevenueCat secret | Server-only REST credential. |
| Billing trust | Client не может self-declare authoritative entitlement. |
| Soft delete | Account status deleted + rewrite normalized email. |

## Rate Limiting

Global:

```text
120 requests / 60 seconds / IP
```

Auth-specific:

| Endpoint | Limit |
|---|---:|
| register | 10/min |
| login | 20/min |
| refresh | 30/min |
| forgot-password | 5/min |
| reset-password | 5/min |
| delete account | 5/min |

## JWT Account Validation

Authenticated requests не полагаются только на token signature/expiry.

`JwtStrategy` проверяет current backend account:

```text
status = active
```

## Billing Trust Boundary

```text
Client purchase state
      ↓
not final authority

Server RevenueCat REST verification
      ↓
normalized subscription state
      ↓
common access decision
```

RevenueCat secret API key остается server-only.

## Webhook Trust Boundary

Webhook request должен передать exact configured authorization header.

Webhook payload — external input.

Event не выдает access напрямую; выполняется RevenueCat REST reconciliation.

## Transport

Production client communication должна использовать HTTPS.

Source description указывает production HTTPS gate на client side; TLS termination/deployment enforcement belongs to operations и здесь не придумывается.

## Not Implemented

Current backend не предоставляет:

- working email verification;
- working password reset;
- 2FA;
- admin HTTP API;
- audit-log UI.

## Secrets

Backend-only environment secrets:

```text
JWT_ACCESS_SECRET
DATABASE_URL
REVENUECAT_SECRET_API_KEY
REVENUECAT_WEBHOOK_AUTH
```

Exact secret-management platform/rotation procedure source не описывает.
