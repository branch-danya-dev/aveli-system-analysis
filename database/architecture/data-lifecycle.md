# Aveli — Data Lifecycle

> Persistent-data behavior across important Aveli product states.

## Core Principle

Aveli separates:

```text
Identity state
Access state
Workspace state
```

A change in one state must not silently destroy data owned by another state.

> **Logout or access expiration may make a workspace inactive or unavailable, but must not delete its persistent professional data.**

## Workspace Lifecycle

```text
Workspace created / first used
        ↓
Professional data created and updated
        ↓
Workspace remains persistent
        ↓
May become inactive after logout
        ↓
May become unavailable after access loss
        ↓
Can become active again for the same user
```

## Logout

`BR-025`, `BR-043`–`BR-046` require the authenticated state to end while persistent workspace information remains.

## Account Switching

`BR-047` requires isolation:

```text
Workspace A inactive
        ↓
User B becomes active
        ↓
Workspace B active
```

No professional workspace data is transferred between users.

## Access Expiration

`BR-026` requires:

```text
Access expires
    ↓
Workspace blocked
    ↓
Persistent data preserved
    ↓
Access restored
    ↓
Same preserved workspace available again
```

## Reinstall / Local Data Loss

The current persistence model has an important boundary:

```text
Reinstall
    ↓
Local application data may be lost
    ↓
Same account signs in
    ↓
A new empty aveli_<userId>.sqlite may be created
```

Server-controlled registration trial state is independent from the local database and is **not reset by reinstall**.

A legacy `aveli.db` file is not automatically attached to an account; without an explicit claim/migration path its data remains unassigned.

This behavior follows from the current local-first ownership model and must be revisited if cloud backup or workspace synchronization is introduced.

## Client Lifecycle

Known rule (`BR-040`):

```text
Active Client
    ↓ archive
Archived Client
    ↓
Historical information remains preserved
```

Permanent client-deletion rules remain a product-level open question.

## Appointment / Visit Lifecycle

Known business states include `Scheduled`, `Cancelled`, `No-show`, and `Completed`.

The physical implementation also contains `confirmed`. Its product significance must be classified before it is promoted into canonical business behavior.

Completed work may carry notes, photos, and payment state.

## Payment Lifecycle

Physical persistence is verified as:

```text
Appointment 1 → 0..1 Payment

PaymentStatus:
unpaid
partial
paid
```

Partial payment is represented in the same row through `amount_paid`.

What remains open is the **business transition policy**: which transitions are allowed, when they occur, and whether any product rules constrain correction/reversal behavior.

## Workspace Media

Visit-photo persistence is split between database metadata and device files:

```text
visit_photos row
    +
documents/visit_photos/<userId>/<appointmentId>/...
```

Deleting an appointment cascades deletion of related `visit_photos` metadata rows.

Physical files are cleaned separately by repository/file-root logic (`VisitPhotosRoot.deleteForUser` is part of the documented implementation behavior).

The remaining open concerns are policy-level guarantees such as orphan cleanup coverage, retention, backup, and recovery behavior.

## Open Lifecycle Questions

- permanent client deletion;
- service deactivation or deletion;
- business rules for payment corrections/transitions;
- import/export conflict behavior;
- orphan-file guarantees and media retention policy;
- backup and restore behavior.

Visible gaps are preferable to invented rules.

## Related Documentation

- [`data-ownership.md`](data-ownership.md)
- [`../local/`](../local/)
- [`../server/`](../server/)
- [`../../business/requirements/business-rules.md`](../../business/requirements/business-rules.md)
- [`../../business/processes/`](../../business/processes/)
