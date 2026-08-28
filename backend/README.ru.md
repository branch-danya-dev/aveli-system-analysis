# Backend

> Каноническая документация Aveli account/access backend.

## Назначение

`backend/` объясняет **чем владеет Aveli server, как разделены internal capabilities, какие contracts он предоставляет, как разрешаются access и billing, и какие server-side security/configuration rules действуют**.

Aveli API намеренно узкий.

Он владеет account, session, access и subscription coordination.

Он **не хранит и не синхронизирует** professional workspace специалиста.

## Статус

**Implementation-verified baseline in progress**

Текущая backend documentation основана на проверенном описании NestJS implementation, API contracts, configuration и runtime behavior.

После применения пакета нужен финальный baseline review.

## Runtime Boundary

```text
Flutter                          NestJS API                    PostgreSQL
────────                         ──────────                    ──────────
AuthRemoteDataSource      →      AuthModule           →        users
AccessRemoteDataSource    →      AccessModule         →        auth_sessions
GET /v1/access                   AccessService                 access_grants
POST /v1/billing/sync     →      BillingModule        →        subscriptions
RevenueCat SDK            ↔      RevenueCat Gateway   →        subscription_events
```

Professional workspace data остаются local.

Canonical ownership:

[`../database/architecture/data-ownership.ru.md`](../database/architecture/data-ownership.ru.md)

## Backend Areas

| Area | Responsibility |
|---|---|
| `architecture/` | Backend capability и trust boundaries. |
| `stack/` | Canonical backend technology knowledge. |
| `api/` | Canonical HTTP contracts и OpenAPI. |
| `auth/` | Registration, sign-in, token/session lifecycle. |
| `access/` | Effective workspace-access decision. |
| `billing/` | RevenueCat reconciliation и subscription snapshot updates. |
| `errors/` | Backend error taxonomy и code ownership. |
| `security/` | Implemented backend security controls. |
| `configuration/` | Runtime environment variables и behavior switches. |
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

Ранее упоминавшийся `/v1/subscription` controller отсутствует в current implementation и не является canonical.

## Путь чтения

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

Начать с:

[`architecture/responsibility-boundary.ru.md`](architecture/responsibility-boundary.ru.md)

Canonical API contract:

[`api/openapi.yaml`](api/openapi.yaml)

## Правила документации

[`../rules.ru.md`](../rules.ru.md)
