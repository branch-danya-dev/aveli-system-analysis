# Backend Security Controls

## Implemented Controls

| Concern | Verified Implementation |
|---|---|
| Password storage | Argon2id hash. |
| Refresh-token storage | SHA-256 hash only. |
| Refresh rotation | Old session revoked on use. |
| Refresh reuse | Reuse of revoked token revokes all active user sessions. |
| Timing leakage | Dummy hash on failed login for missing user. |
| Access JWT | JWT bearer token with account-status validation. |
| HTTP headers | `helmet`. |
| Body validation | Whitelist + reject unknown fields. |
| Rate limiting | Global + auth-route throttles. |
| Webhook auth | Exact shared-secret Authorization header. |
| RevenueCat secret | Server-only REST credential. |
| Billing trust | Client cannot self-declare entitlement as authoritative. |
| Soft delete | Account status set deleted and normalized email rewritten. |

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

Authenticated requests do not rely only on token signature/expiry.

`JwtStrategy` verifies the current backend account is:

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

The RevenueCat secret API key must remain server-only.

## Webhook Trust Boundary

Webhook requests must present the exact configured authorization header value.

Webhook payload data is treated as external input.

The event itself does not directly grant access; reconciliation is performed through RevenueCat REST.

## Transport

Production client communication is expected to use HTTPS.

The supplied implementation description identifies the production HTTPS gate on the client side; TLS termination/deployment enforcement details belong to deployment/operations documentation and are not inferred here.

## Not Implemented

Current backend does not provide:

- working email verification;
- working password reset;
- 2FA;
- admin HTTP API;
- audit-log UI.

These absences should remain visible.

## Secrets

Backend-only environment secrets include:

```text
JWT_ACCESS_SECRET
DATABASE_URL
REVENUECAT_SECRET_API_KEY
REVENUECAT_WEBHOOK_AUTH
```

Exact secret-management platform/rotation procedure is not documented by the current source description.
