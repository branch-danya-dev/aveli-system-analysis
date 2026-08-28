# `payments`

> Не более одной физической строки оплаты на одну запись клиента.

| Столбец | Тип | NULL | Назначение |
|---|---|---|---|
| `id` | TEXT | NO | Первичный ключ. |
| `appointment_id` | TEXT | NO | Внешний ключ → `appointments.id`; `UNIQUE`; `ON DELETE CASCADE`. |
| `amount` | INTEGER | NO | Сумма к оплате в целых единицах. |
| `amount_paid` | INTEGER | NO | Оплаченная сумма; по умолчанию 0; добавлена в v6. |
| `status` | TEXT | NO | `PaymentStatus`. |
| `method` | TEXT | YES | `PaymentMethod`; добавлен в v3. |
| `created_at` | DATETIME | NO | Время создания. |
| `paid_at` | DATETIME | YES | Время полной или частичной оплаты; добавлено в v6. |

## Инвариант

`UNIQUE(appointment_id)` гарантирует не более одной строки оплаты для одной записи.

Частичная оплата хранится в этой же строке:

```text
amount
amount_paid
status = partial
```

`appointments.payment_status` — намеренная агрегированная копия состояния оплаты для чтения интерфейсом.
