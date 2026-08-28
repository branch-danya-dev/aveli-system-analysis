# `payments`

> One physical payment row per appointment.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK. |
| `appointment_id` | TEXT | NO | FK → `appointments.id`; UNIQUE; ON DELETE CASCADE. |
| `amount` | INTEGER | NO | Amount due in whole currency units. |
| `amount_paid` | INTEGER | NO | Amount paid; default 0; added in v6. |
| `status` | TEXT | NO | `PaymentStatus`. |
| `method` | TEXT | YES | `PaymentMethod`; added in v3. |
| `created_at` | DATETIME | NO | Creation timestamp. |
| `paid_at` | DATETIME | YES | Time recorded for full/partial payment; added in v6. |

## Invariant

`UNIQUE(appointment_id)` enforces at most one payment row per appointment.

Partial payment is represented in the same row:

```text
amount
amount_paid
status = partial
```

`appointments.payment_status` is an intentional aggregate copy for UI-oriented reads.
