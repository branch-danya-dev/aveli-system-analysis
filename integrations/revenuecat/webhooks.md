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

must exactly equal configured:

```text
REVENUECAT_WEBHOOK_AUTH
```

This is a shared-secret webhook boundary, not JWT client authentication.

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

Canonical idempotency key:

```text
subscription_events.external_event_id
```

A processed duplicate is skipped.

## Persisted Event Evidence

The backend stores:

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

must match the expected Aveli UUID format before server reconciliation is attempted.

Invalid/non-Aveli ids are not used to resolve another account.

## Critical Access Rule

Webhook `event_type` does **not** directly grant/revoke Aveli workspace access.

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

- records `processingError`;
- rethrows;
- produces non-2xx behavior to the provider caller.

Success:

```json
{ "ok": true }
```

HTTP 200.

Backend-local implementation:

[`../../backend/billing/webhook-processing.md`](../../backend/billing/webhook-processing.md)
