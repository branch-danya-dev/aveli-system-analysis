# Backend

> Canonical documentation for the Aveli account/access backend.

## Status

**Baseline: Stable**

The backend documentation is reconciled with verified NestJS implementation evidence, API contracts, persistence, access logic, billing integration, and final whole-system review.

## Responsibility

The backend owns:

```text
account identity
authentication
refresh sessions
trial / manual / lifetime grants
normalized subscription state
effective online workspace access
billing reconciliation
RevenueCat webhook processing
```

It does **not** store or synchronize the professional workspace.

## Areas

| Area | Responsibility |
|---|---|
| `architecture/` | Backend responsibility/trust boundary. |
| `stack/` | Backend runtime technologies. |
| `api/` | Canonical HTTP contracts/OpenAPI. |
| `auth/` | Registration/login/session lifecycle. |
| `access/` | Effective access decision. |
| `billing/` | RevenueCat reconciliation/subscription snapshot. |
| `errors/` | Backend error taxonomy. |
| `security/` | Implemented server security controls. |
| `configuration/` | Runtime environment/configuration. |
| `admin/` | Non-HTTP administrative CLI. |

## HTTP Surface

```text
GET    /health
GET    /ready
POST   /v1/auth/register
POST   /v1/auth/login
POST   /v1/auth/refresh
POST   /v1/auth/logout
POST   /v1/auth/logout-all
GET    /v1/auth/me
DELETE /v1/auth/me
POST   /v1/auth/resend-verification   # 501 stub
POST   /v1/auth/verify-email          # 501 stub
POST   /v1/auth/forgot-password       # 501 stub
POST   /v1/auth/reset-password        # 501 stub
GET    /v1/access
POST   /v1/billing/sync
POST   /v1/webhooks/revenuecat
```

501 routes are future contract stubs, not shipped end-to-end product capabilities.

Canonical API: [`api/openapi.yaml`](api/openapi.yaml)

Data ownership: [`../database/architecture/data-ownership.md`](../database/architecture/data-ownership.md)

## Documentation Rules

[`../rules.md`](../rules.md)
