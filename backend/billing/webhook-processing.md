# RevenueCat Webhook Processing

> Verified server-to-server processing model for RevenueCat lifecycle events.

## Endpoint

```text
POST /v1/webhooks/revenuecat
```

## Authentication

The incoming `Authorization` header must exactly equal configured:

```text
REVENUECAT_WEBHOOK_AUTH
```

This shared-secret check is separate from JWT client authentication.

## Processing

Verified flow:

1. extract `event.id` as `external_event_id`, or derive a synthetic hash when required;
2. check idempotency;
3. if an existing event is already processed, skip repeated processing;
4. insert/update `subscription_events` with sanitized payload;
5. if `app_user_id` is a valid Aveli UUID, reconcile that user through `syncCustomer(userId)`;
6. mark `processed_at` on success or `processing_error` on failure.

## Critical Rule

The webhook does **not** grant or revoke workspace access directly from `event_type`.

Instead:

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

This reduces dependence on individual event-order/type interpretation.

## Idempotency

Canonical persistence key:

```text
subscription_events.external_event_id
```

Database uniqueness supports webhook idempotency.

See:

[`../../database/server/entities/subscription_events.md`](../../database/server/entities/subscription_events.md)

## Payload

Raw provider payload is stored as JSONB after sanitization according to current implementation behavior.

Its internal structure is not a stable client contract.

## Failure Persistence

Processing failures may be retained in:

```text
processing_error
```

and successful processing time in:

```text
processed_at
```
