# `subscription_events`

> Persisted records обработки subscription webhook/events.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Идентификатор event row. |
| `subscription_id` | UUID FK nullable | Связанная `subscriptions` row; ON DELETE SET NULL. |
| `user_id` | UUID FK nullable | Связанный `users` record; ON DELETE SET NULL. |
| `provider` | TEXT | Provider события; обычно `revenuecat`. |
| `external_event_id` | TEXT UNIQUE | Внешний idempotency key webhook event. |
| `event_type` | TEXT | Тип provider event. |
| `payload` | JSONB | Raw provider payload. |
| `received_at` | TIMESTAMPTZ | Время получения event. |
| `processed_at` | TIMESTAMPTZ nullable | Время успешного/завершенного processing. |
| `processing_error` | TEXT nullable | Ошибка processing, если возникла. |

## Invariant

`external_event_id` имеет UNIQUE и является persistence-level основой webhook idempotency.

Nullable FKs с `SET NULL` позволяют сохранять event history после удаления связанного user или subscription.

## Связанная документация

- [`../constraints/invariants.ru.md`](../constraints/invariants.ru.md)
