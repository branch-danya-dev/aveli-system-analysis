# `appointments`

> Central physical scheduling record.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK. |
| `client_id` | TEXT | NO | FK → `clients.id`; no ON DELETE CASCADE documented. |
| `service_id` | TEXT | NO | FK → `services.id`. |
| `starts_at` | DATETIME | NO | Slot start. |
| `ends_at` | DATETIME | NO | Slot end. |
| `price` | INTEGER | NO | Price snapshot at booking time. |
| `status` | TEXT | NO | `AppointmentStatus`. |
| `payment_status` | TEXT | NO | Aggregated `PaymentStatus` used by UI. |
| `created_at` | DATETIME | NO | Creation timestamp. |
| `updated_at` | DATETIME | NO | Update timestamp. |

## Indexes

- `appointments_starts_at(starts_at)` — calendar / Today.
- `appointments_client_id(client_id)` — client history.

## Denormalization

`price` is intentionally copied from Service at booking time so later service-price changes do not rewrite historical appointments.
