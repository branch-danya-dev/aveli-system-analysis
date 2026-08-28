# Aveli Backend — Responsibility Boundary

> Verified backend-local capability, trust, and ownership boundaries.

## Backend Mission

Aveli backend is a narrow account service.

It answers:

```text
Who is this account?
        +
May this authenticated account open the workspace now?
```

It also reconciles RevenueCat subscription evidence required by the access decision.

## Verified NestJS Modules

```text
AppModule
├── ConfigModule
├── ThrottlerModule
├── PrismaModule
├── HealthModule
├── AuthModule
├── AccessModule
└── BillingModule
```

Application bootstrap in `backend/src/main.ts` also applies:

- `helmet`;
- global `ValidationPipe`;
- optional CORS.

Global HTTP error serialization is owned by `ApiExceptionFilter`.

## Capability Map

### AuthModule

Owns:

- registration;
- sign-in;
- JWT authentication;
- refresh-session lifecycle;
- logout/session revocation;
- account self-read/delete endpoints.

### AccessModule

Owns:

- `GET /v1/access`;
- access-source loading;
- deterministic access resolution;
- non-HTTP `AdminAccessService`.

The pure decision implementation is located in:

```text
backend/src/access/access.decision.ts
```

### BillingModule

Owns:

- `POST /v1/billing/sync`;
- `POST /v1/webhooks/revenuecat`;
- RevenueCat REST reconciliation;
- subscription snapshot persistence;
- webhook-event processing.

### HealthModule

Owns:

```text
GET /health
GET /ready
```

`/ready` verifies PostgreSQL connectivity.

## Explicit Non-Responsibility

The backend does not store or synchronize:

```text
clients
appointments
services
visit notes
visit photos
professional payments
local schedule
local UI profile
```

This is an architectural boundary, not merely a missing endpoint set.

## Data Boundary

Backend-owned persistence:

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

Canonical physical model:

[`../../database/server/`](../../database/server/)

## API Boundary

The canonical current API consists of:

```text
/v1/auth/*
GET  /v1/access
POST /v1/billing/sync
POST /v1/webhooks/revenuecat
```

plus unversioned health/readiness endpoints.

A `/v1/subscription` controller is not present in the current code and must not be documented as current behavior.

## RevenueCat Trust Boundary

The mobile client does not submit a self-declared entitlement boolean to grant access.

Billing sync uses the authenticated server user id:

```text
JWT sub
   ↓
RevenueCat REST lookup
   ↓
Normalized subscription snapshot
   ↓
Common access decision
```

Webhooks also reconcile through RevenueCat REST instead of granting access directly from event type.

## Cross-Layer Consequence

```text
Backend Access = denied
        ↓
Workspace unavailable

NOT

Backend Access = denied
        ↓
Workspace deleted
```

The backend controls availability, not ownership of professional workspace data.

## Out of Scope

Current backend intentionally lacks:

- cloud workspace sync;
- server-side workspace entities;
- public booking backend;
- server-side visit-media storage;
- normal workspace push notifications;
- admin HTTP API;
- 2FA;
- implemented email verification;
- implemented password reset.

## Related Documentation

- [`../api/`](../api/)
- [`../auth/`](../auth/)
- [`../access/`](../access/)
- [`../billing/`](../billing/)
- [`../../database/`](../../database/)
