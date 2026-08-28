# Database Implementation Verification Notes

> Отличия и подтверждения, найденные при сравнении logical model с предоставленным physical persistence description.

## Подтверждено

- local workspace использует отдельную SQLite database на authenticated user;
- local business entity ids — UUID v4;
- local entities не имеют FK на server user UUID;
- appointment имеет максимум одну physical payment row;
- partial payment хранится через `amount_paid` в этой строке;
- appointment хранит start **и end** timestamps;
- appointment хранит price snapshot;
- appointment также хранит denormalized `payment_status`;
- visit-photo metadata хранит ссылки и на appointment, и на client;
- app settings физически хранятся как key-value TEXT;
- server access grants и subscription state физически разделены;
- uniqueness registration trial enforced в PostgreSQL;
- subscription state — RevenueCat snapshot, raw events хранятся отдельно.

## Корректировки Logical Model

Теперь как verified можно считать:

```text
Appointment 1 → 0..1 Payment
```

Также logical model следует уточнить:

```text
Appointment.endsAt
Appointment.priceSnapshot
Appointment.paymentStatus (derived/aggregated logical state)
Service.returnInterval
Payment.amountPaid
Payment.paidAt
```

Часть этих attributes подтверждена implementation и требует review business traceability.

## Traceability Gaps, обнаруженные physical persistence

Physical schema содержит понятия, пока слабо отраженные в business documentation:

- service return interval;
- profile public slug / public listing;
- local phone/email verification flags;
- account lifecycle UI setting;
- explicit exchange-rate cache persistence;
- `confirmed` appointment status;
- server `deleted` / `disabled` user states;
- store/provider `trialing`, `past_due`, `grace_period`, `revoked` subscription statuses.

Это не обязательно ошибки business model. Их нужно классифицировать как:

```text
current product requirement
implementation-only support state
legacy/future capability
```

до того, как поднимать их в business documentation.

## Source Naming Discrepancy

В local ER overview используются:

```text
duration_minutes
return_interval_minutes
```

а detailed table section использует:

```text
duration
return_interval
```

Перед generated exact SQLite DDL contract нужна прямая сверка с source code.
