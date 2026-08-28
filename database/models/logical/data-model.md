# Aveli — Logical Data Model

> Technology-independent logical representation of the information Aveli must preserve.

---

## Purpose

The logical model refines the conceptual domain into attributes, identifiers, relationships, and cardinalities required by the current product behavior.

It is informed by the verified physical persistence model, but remains independent of SQLite, PostgreSQL, Drift, Prisma, and concrete SQL types.

---

## Model Status

This document is now a **verified baseline logical model**.

The following implementation facts have been confirmed by the current persistence model:

- one physical payment row per appointment;
- partial payment is represented inside that row;
- appointments preserve both start and end timestamps;
- appointments preserve a price snapshot;
- appointments preserve an aggregated payment status;
- services preserve a return interval;
- visit-photo metadata references both appointment and client;
- workspace settings are physically persisted as key-value values;
- access grants and subscription state are separate backend persistence concepts.

Physical implementation may still contain technical fields not promoted into this logical model.

---

# Identity & Access Domain

## Account

Represents the persistent Aveli identity.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable account identifier. |
| `email` | Account sign-in identity where email authentication is used. |
| `status` | Account lifecycle state when required by product/backend behavior. |

Relationships:

```text
Account 1 → 0..* Session
Account 1 → 0..* Access Source
Account 1 → 0..* Subscription State
```

Professional workspace entities remain outside the backend account ownership domain.

---

## Session

Represents authenticated session state associated with one Account.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable session identifier. |
| `accountId` | Owning Account. |
| `validUntil` | Session validity boundary. |
| `state` | Session lifecycle state where needed. |
| `deviceContext` | Optional device/platform context associated with the session. |

Exact token hashes and storage mechanics are physical/backend concerns.

---

## Access Source

Represents a persisted source that may contribute to effective workspace access.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable source identifier. |
| `accountId` | Account receiving the source. |
| `type` | Lifetime, Manual Grant, Trial, or another supported source. |
| `source` | Origin of the grant where relevant. |
| `startsAt` | Beginning of validity when applicable. |
| `expiresAt` | End of validity when applicable. |
| `revokedAt` | Early revocation where supported. |

Effective `Access` remains a derived concept rather than a required standalone persisted entity.

---

## Subscription State

Represents subscription-backed access information known to Aveli.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable subscription-state identifier where persisted independently. |
| `accountId` | Related Account. |
| `product` | Store product reference. |
| `entitlement` | Logical entitlement. |
| `status` | Current subscription state. |
| `autoRenew` | Auto-renew state. |
| `validUntil` | Current access/subscription validity boundary where applicable. |
| `lastVerifiedAt` | Last provider verification timestamp where tracked. |

Provider-specific raw payloads are not part of the logical subscription model.

---

## Subscription Event

Represents an externally received subscription event retained for processing/audit/idempotency.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable event identifier. |
| `subscriptionId` | Related Subscription State where resolvable. |
| `accountId` | Related Account where resolvable. |
| `externalEventId` | External idempotency identifier. |
| `eventType` | Provider event type. |
| `payload` | Raw provider event content. |
| `receivedAt` | Receipt time. |
| `processedAt` | Processing completion time where available. |
| `processingError` | Processing failure detail where present. |

This entity is backend-oriented and does not change professional workspace ownership.

---

# Professional Workspace Domain

## Client

Represents a professional client.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable client identifier inside the workspace. |
| `name` | Client display name. |
| `phone` | Phone/contact value where available. |
| `social` | Social/messenger contact where available. |
| `deviceContactId` | Imported device-contact reference where available. |
| `tags` | Client classification metadata. |
| `archivedAt` | Archive timestamp where archived. |
| `createdAt` | Creation timestamp. |

Relationship:

```text
Client 1 → 0..* Appointment
```

Client history is derived from associated professional activity.

---

## Service

Represents a type of professional work.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable service identifier. |
| `name` | Service name. |
| `duration` | Expected duration used for planning. |
| `price` | Current default service price. |
| `returnInterval` | Suggested/expected return interval where configured. |

Relationship:

```text
Service 1 → 0..* Appointment
```

The final service delete/deactivate lifecycle remains a business-rule concern.

---

## Appointment

Represents planned professional work and the central scheduling record.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable appointment identifier. |
| `clientId` | Related Client. |
| `serviceId` | Related Service. |
| `startsAt` | Planned start date/time. |
| `endsAt` | Planned end date/time. |
| `priceSnapshot` | Price preserved for this appointment independently of later Service price changes. |
| `status` | Current appointment lifecycle state. |
| `paymentStatus` | Aggregated payment state used by appointment-oriented behavior. |
| `createdAt` | Creation timestamp. |
| `updatedAt` | Last update timestamp. |

Relationships:

```text
Client      1 → 0..* Appointment
Service     1 → 0..* Appointment
Appointment 1 → 0..* Visit Note
Appointment 1 → 0..* Visit Photo
Appointment 1 → 0..1 Payment
```

`Appointment 1 → 0..1 Payment` is now verified by the physical unique constraint.

The conceptual `Visit` remains a valid business/domain concept even though the persistence model represents visit-related data through appointment-associated records.

---

## Visit Note

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable note identifier. |
| `appointmentId` | Appointment/visit context. |
| `body` | Note content. |
| `createdAt` | Creation timestamp. |

Relationship:

```text
Appointment 1 → 0..* Visit Note
```

---

## Visit Photo

Represents metadata for media associated with visit context.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable photo metadata identifier. |
| `appointmentId` | Appointment/visit context. |
| `clientId` | Related Client, preserved for direct client-oriented retrieval. |
| `mediaReference` | Logical reference to the stored file. |
| `type` | Before / after / general classification. |
| `createdAt` | Creation timestamp. |

The physical file path is intentionally not part of the technology-independent logical model.

---

## Payment

Represents one payment record associated with one appointment.

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable payment identifier. |
| `appointmentId` | Related Appointment. |
| `amount` | Amount due. |
| `amountPaid` | Amount already paid. |
| `status` | Unpaid / partial / paid state. |
| `method` | Payment method where recorded. |
| `createdAt` | Creation timestamp. |
| `paidAt` | Full/partial payment timestamp where available. |

Verified cardinality:

```text
Appointment 1 → 0..1 Payment
```

Partial payment is represented by values inside the single Payment record rather than by multiple Payment rows.

---

## Workspace Settings

Represents workspace/application settings persisted for local behavior.

Known logical groups include:

- specialist profile values;
- working currency;
- locale;
- appearance/theme;
- working schedule;
- reminder enablement;
- supported local/public-profile state;
- exchange-rate cache state.

This is a logical grouping only. It does not require a normalized settings entity or table structure.

Some keys may represent implementation-only or future-facing capability and should not automatically be promoted into business requirements.

---

# Logical Relationships

```text
IDENTITY & ACCESS

Account
 ├── 0..* Session
 ├── 0..* Access Source
 ├── 0..* Subscription State
 └── 0..* Subscription Event

Effective Access
    ↑ derived from Access Source + Subscription State
    │
    └── controls workspace availability


PROFESSIONAL WORKSPACE

Client
  1
  └── 0..* Appointment

Service
  1
  └── 0..* Appointment

Appointment
  1
  ├── 0..* Visit Note
  ├── 0..* Visit Photo
  └── 0..1 Payment

Workspace Settings
  └── affects local workspace/application behavior
```

---

# Separation from Physical Persistence

This model still does not define:

```text
UUID vs TEXT id storage
SQLite vs PostgreSQL type choices
index names
CHECK syntax
CASCADE syntax
key-value serialization format
absolute file paths
ORM-specific models
```

Those remain canonical in the physical persistence documentation.

---

# Open / Classification Questions

The physical implementation exposed several concepts that require classification before being promoted upward:

- `confirmed` appointment status;
- service `returnInterval` business significance;
- profile public slug / public listing;
- local phone/email verification flags;
- local account lifecycle setting;
- exchange-rate cache persistence;
- backend `disabled` / `deleted` account states;
- provider subscription states such as `past_due`, `grace_period`, `revoked`, `trialing`.

Each should be classified as one of:

```text
current product requirement
implementation support state
legacy/future capability
```

---

## Related Documentation

- [`../conceptual/domain-model.md`](../conceptual/domain-model.md)
- [`../../architecture/data-ownership.md`](../../architecture/data-ownership.md)
- [`../../architecture/data-lifecycle.md`](../../architecture/data-lifecycle.md)
- [`../../local/`](../../local/)
- [`../../server/`](../../server/)
