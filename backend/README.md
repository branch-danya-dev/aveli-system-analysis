# Backend

> Canonical documentation for the Aveli account/access backend.

## Purpose

`backend/` explains what the Aveli server owns, how its internal capabilities are separated, which contracts it exposes, how access and billing are resolved, and which server-side security/configuration rules apply.

Aveli API is intentionally narrow. It owns account, session, access and subscription coordination. It does **not** store or synchronize the specialist's professional workspace.

## Status

**Implementation-verified baseline in progress**

The current backend documentation is grounded in the verified NestJS implementation description, API contracts, configuration and runtime behavior. A final baseline review should be performed after this correction pass is merged.

## Runtime Boundary

```text
Flutter Client
   │
   ├── AuthRemoteDataSource
   │        ↓
   │     AuthModule
   │        ↓
   │     users + auth_sessions
   │
   ├── AccessRemoteDataSource
   │        ↓
   │     AccessModule / AccessService
   │        ↓
   │     access_grants + subscriptions
   │
   └── POST /v1/billing/sync
            ↓
         BillingModule
            ↓
         subscriptions + subscription_events
            ↕
        RevenueCat REST / webhooks
```

Professional workspace data remains local.

Canonical ownership:

[`../database/architecture/data-ownership.md`](../database/architecture/data-ownership.md)

## Backend Areas

| Area | Responsibility |
|---|---|
| `architecture/` | Backend capability and trust boundaries. |
| `stack/` | Canonical backend technology knowledge. |
| `api/` | Canonical HTTP contracts and OpenAPI. |
| `auth/` | Registration, sign-in, token/session lifecycle. |
| `access/` | Effective workspace-access decision. |
| `billing/` | RevenueCat reconciliation and subscription snapshot updates. |
| `errors/` | Backend error taxonomy and code ownership. |
| `security/` | Implemented backend security controls. |
| `configuration/` | Runtime environment variables and behavior switches. |
| `admin/` | Non-HTTP administrative CLI behavior. |

## Known HTTP Surface

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

The previously mentioned `/v1/subscription` controller is not part of the current implementation and must not be treated as canonical.

## Reading Path

```text
architecture/
    ↓
stack/
    ↓
api/
    ↓
auth/ + access/ + billing/
    ↓
errors/ + security/ + configuration/
```

Start with [`architecture/responsibility-boundary.md`](architecture/responsibility-boundary.md).

Canonical API contract: [`api/openapi.yaml`](api/openapi.yaml).

## Documentation Rules

[`../rules.md`](../rules.md)
