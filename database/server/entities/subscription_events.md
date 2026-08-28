# `subscription_events`

> Persisted subscription webhook/event processing records.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Event-row identifier. |
| `subscription_id` | UUID FK nullable | → subscriptions; ON DELETE SET NULL. |
| `user_id` | UUID FK nullable | → users; ON DELETE SET NULL. |
| `provider` | TEXT | Usually revenuecat. |
| `external_event_id` | TEXT UNIQUE | External idempotency key. |
| `event_type` | TEXT | Provider event type. |
| `payload` | JSONB | Raw provider payload. |
| `received_at` | TIMESTAMPTZ | Receive time. |
| `processed_at` | TIMESTAMPTZ nullable | Processing completion. |
| `processing_error` | TEXT nullable | Processing error. |

## Invariant

`external_event_id` is UNIQUE and supports webhook idempotency.

Nullable FKs with `SET NULL` allow event history to remain after related user/subscription deletion.
