# RevenueCat — Webhook Boundary

## Endpoint

```text
POST /v1/webhooks/revenuecat
```

Backend service:

```text
RevenueCatWebhookService
```

## Authentication

Incoming header:

```text
Authorization
```

должен точно равняться configured:

```text
REVENUECAT_WEBHOOK_AUTH
```

Это shared-secret webhook boundary, не JWT client authentication.

## Event Identity

Primary external id:

```text
event.id
```

Fallback synthetic id:

```text
noid:<sha256(JSON...)>
```

## Idempotency

Canonical key:

```text
subscription_events.external_event_id
```

Processed duplicate пропускается.

## Persisted Event Evidence

Backend сохраняет:

```text
provider
eventType
userId
sanitized payload
receivedAt
processedAt
processingError
```

## User Mapping

Webhook:

```text
event.app_user_id
```

должен соответствовать expected Aveli UUID format до server reconciliation.

Invalid/non-Aveli ids не используются для resolve другого account.

## Critical Access Rule

Webhook `event_type` **не** выдает/revokes Aveli workspace access напрямую.

```text
Webhook
  ↓
identify Aveli user
  ↓
RevenueCat REST reconciliation
  ↓
normalized subscription
  ↓
Aveli access decision
```

## Failure Semantics

Processing failure:

- writes `processingError`;
- rethrows;
- приводит к non-2xx provider response.

Success:

```json
{ "ok": true }
```

HTTP 200.

Backend-local implementation:

[`../../backend/billing/webhook-processing.ru.md`](../../backend/billing/webhook-processing.ru.md)
