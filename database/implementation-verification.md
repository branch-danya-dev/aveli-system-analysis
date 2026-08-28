# Database Implementation Verification

> Record of the reconciliation between the logical data model and the supplied physical persistence description.

## Status

**Applied**

The verified implementation facts listed below have already been incorporated into the current logical and physical database documentation.

This file is an audit/verification record, not another canonical copy of the data model.

## Applied Verification Results

The following facts were confirmed and applied:

- local workspace uses one SQLite database per authenticated user;
- local business entity identifiers are UUID v4;
- local entities do not contain foreign keys to the server user UUID;
- one appointment has at most one physical payment row;
- partial payment is represented through `amount_paid` in that row;
- appointments preserve both start and end timestamps;
- appointments preserve a service-price snapshot;
- appointments preserve denormalized `payment_status`;
- Service persistence includes a return interval;
- visit-photo metadata stores appointment and client references;
- app settings use key-value text persistence;
- server access grants and subscription state are physically separate;
- registration-trial uniqueness is enforced in PostgreSQL;
- subscription state is stored as a RevenueCat snapshot while raw events are persisted separately.

Canonical results now live in:

- [`models/logical/data-model.md`](models/logical/data-model.md)
- [`local/`](local/)
- [`server/`](server/)

## Findings Requiring Product Classification

Physical persistence contains concepts that are not yet strongly represented in business documentation:

- `confirmed` appointment status;
- business significance of Service `returnInterval`;
- profile public slug / public listing;
- local phone/email verification flags;
- local account lifecycle setting;
- exchange-rate cache persistence;
- backend `disabled` / `deleted` user states;
- provider subscription states such as `trialing`, `past_due`, `grace_period`, and `revoked`.

Each should be classified before promotion into business documentation:

```text
current product requirement
implementation support state
legacy/future capability
```

These findings do not block the database baseline because the physical state is documented without inventing business meaning.

## Known Source Naming Discrepancy

The supplied local persistence description contains two naming forms for Service duration fields:

```text
ER overview:      duration_minutes / return_interval_minutes
Detailed table:  duration / return_interval
```

Current physical documentation follows the detailed table definition:

```text
duration
return_interval
```

The discrepancy remains explicitly visible until the exact Drift table declarations are checked directly.

It affects naming certainty, not the logical meaning of the fields.

## Baseline Result

The database branch is considered **Stable / baseline-ready with one documented source naming discrepancy**.

Future implementation changes should update the owning physical document first and then trigger logical/traceability review where required.
