# RevenueCat Webhook Processing

> Проверенная server-to-server processing model RevenueCat lifecycle events.

## Endpoint

```text
POST /v1/webhooks/revenuecat
```

## Authentication

Incoming `Authorization` header должен точно равняться:

```text
REVENUECAT_WEBHOOK_AUTH
```

Shared-secret check отделен от JWT client authentication.

## Processing

Verified flow:

1. извлечь `event.id` как `external_event_id` или создать synthetic hash;
2. проверить idempotency;
3. если existing event уже processed — skip;
4. insert/update `subscription_events` с sanitized payload;
5. если `app_user_id` — valid Aveli UUID, reconcile через `syncCustomer(userId)`;
6. set `processed_at` при success или `processing_error` при failure.

## Critical Rule

Webhook **не** выдает и не отбирает workspace access напрямую по `event_type`.

Вместо этого:

```text
Webhook Event
      ↓
Identify Aveli user
      ↓
RevenueCat REST reconciliation
      ↓
Normalized subscription snapshot
      ↓
Common access decision
```

## Idempotency

Canonical persistence key:

```text
subscription_events.external_event_id
```

Database uniqueness поддерживает webhook idempotency.

См.:

[`../../database/server/entities/subscription_events.ru.md`](../../database/server/entities/subscription_events.ru.md)

## Payload

Raw provider payload сохраняется как JSONB после sanitization согласно current implementation behavior.

Его structure не является stable client contract.

## Failure Persistence

Processing failure может сохраняться в:

```text
processing_error
```

successful processing time:

```text
processed_at
```
