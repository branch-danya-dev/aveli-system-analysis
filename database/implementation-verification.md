# Database Implementation Verification Notes

> Differences and confirmations discovered when the logical model was compared with the supplied physical persistence description.

## Confirmed

- local workspace uses one SQLite database per authenticated user;
- local business entity ids are UUID v4;
- local entities have no FK to the server user UUID;
- one appointment has at most one physical payment row;
- partial payment is represented through `amount_paid` in that row;
- appointment stores start **and end** timestamps;
- appointment stores a price snapshot;
- appointment also stores denormalized `payment_status`;
- visit-photo metadata stores both appointment and client references;
- app settings are physically stored as key-value text;
- server access grants and subscription state are physically separate;
- registration trial uniqueness is enforced in PostgreSQL;
- subscription state is a RevenueCat snapshot, while raw events are separately persisted.

## Logical Model Corrections to Apply

The logical model should now treat the following as verified rather than provisional:

```text
Appointment 1 → 0..1 Payment
```

It should also include/clarify:

```text
Appointment.endsAt
Appointment.priceSnapshot
Appointment.paymentStatus (derived/aggregated logical state)
Service.returnInterval
Payment.amountPaid
Payment.paidAt
```

Some of these are implementation-confirmed attributes whose business traceability should be reviewed.

## Traceability Gaps Exposed by Physical Persistence

The physical schema contains concepts not yet strongly represented in business documentation, including:

- service return interval;
- profile public slug / public listing;
- local phone/email verification flags;
- account lifecycle UI setting;
- explicit exchange-rate cache persistence;
- `confirmed` appointment status;
- server `deleted` / `disabled` user states;
- store/provider `trialing`, `past_due`, `grace_period`, and `revoked` subscription statuses.

These are not necessarily business-model errors. They are candidates for one of three classifications:

```text
current product requirement
implementation-only support state
legacy/future capability
```

They should be classified before being promoted upward into business documentation.

## Source Naming Discrepancy

The supplied document's local ER overview uses:

```text
duration_minutes
return_interval_minutes
```

while its detailed table section uses:

```text
duration
return_interval
```

Direct source-code verification is required before generating an exact SQLite DDL contract.
