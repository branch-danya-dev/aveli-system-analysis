# Aveli Backend — Responsibility Boundary

> Проверенные backend-local capability, trust и ownership boundaries.

## Миссия Backend

Aveli backend — узкий account service.

Он отвечает:

```text
Кто этот account?
        +
Может ли этот authenticated account открыть workspace сейчас?
```

Также он reconciles RevenueCat subscription evidence, необходимый access decision.

## Проверенные NestJS Modules

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

Application bootstrap в `backend/src/main.ts` также применяет:

- `helmet`;
- global `ValidationPipe`;
- optional CORS.

Global HTTP error serialization принадлежит `ApiExceptionFilter`.

## Capability Map

### AuthModule

Владеет:

- registration;
- sign-in;
- JWT authentication;
- refresh-session lifecycle;
- logout/session revocation;
- account self-read/delete endpoints.

### AccessModule

Владеет:

- `GET /v1/access`;
- загрузкой access sources;
- deterministic access resolution;
- non-HTTP `AdminAccessService`.

Pure decision implementation:

```text
backend/src/access/access.decision.ts
```

### BillingModule

Владеет:

- `POST /v1/billing/sync`;
- `POST /v1/webhooks/revenuecat`;
- RevenueCat REST reconciliation;
- subscription snapshot persistence;
- webhook-event processing.

### HealthModule

Владеет:

```text
GET /health
GET /ready
```

`/ready` проверяет PostgreSQL connectivity.

## Explicit Non-Responsibility

Backend не хранит и не синхронизирует:

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

Это architectural boundary, а не просто отсутствующий набор endpoints.

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

Canonical current API:

```text
/v1/auth/*
GET  /v1/access
POST /v1/billing/sync
POST /v1/webhooks/revenuecat
```

плюс unversioned health/readiness endpoints.

`/v1/subscription` controller отсутствует в current code и не должен документироваться как current behavior.

## RevenueCat Trust Boundary

Mobile client не передает self-declared entitlement boolean для выдачи access.

Billing sync использует authenticated server user id:

```text
JWT sub
   ↓
RevenueCat REST lookup
   ↓
Normalized subscription snapshot
   ↓
Common access decision
```

Webhooks также reconciles через RevenueCat REST, а не выдают access напрямую по event type.

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

Backend контролирует availability, а не ownership professional workspace data.

## Out of Scope

Current backend намеренно не имеет:

- cloud workspace sync;
- server-side workspace entities;
- public booking backend;
- server-side visit-media storage;
- normal workspace push notifications;
- admin HTTP API;
- 2FA;
- implemented email verification;
- implemented password reset.

## Связанная документация

- [`../api/`](../api/)
- [`../auth/`](../auth/)
- [`../access/`](../access/)
- [`../billing/`](../billing/)
- [`../../database/`](../../database/)
