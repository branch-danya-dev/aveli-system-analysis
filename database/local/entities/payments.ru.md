# `payments`

> Одна physical payment row на appointment.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK. |
| `appointment_id` | TEXT | NO | FK → `appointments.id`; UNIQUE; ON DELETE CASCADE. |
| `amount` | INTEGER | NO | Сумма к оплате в целых единицах. |
| `amount_paid` | INTEGER | NO | Оплаченная сумма; default 0; v6. |
| `status` | TEXT | NO | `PaymentStatus`. |
| `method` | TEXT | YES | `PaymentMethod`; v3. |
| `created_at` | DATETIME | NO | Создание. |
| `paid_at` | DATETIME | YES | Время полной/частичной оплаты; v6. |

## Invariant

`UNIQUE(appointment_id)` гарантирует максимум одну payment row на appointment.

Partial payment хранится в той же строке:

```text
amount
amount_paid
status = partial
```

`appointments.payment_status` — намеренная агрегированная копия для UI-oriented reads.
