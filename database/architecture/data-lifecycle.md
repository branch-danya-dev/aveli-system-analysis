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

No workspace data is transferred between users.

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
Same workspace available again
```

## Client Lifecycle

Known rule (`BR-040`):

```text
Active Client
    ↓ archive
Archived Client
    ↓
Historical information remains preserved
```

Permanent deletion rules are not yet final and must not be invented here.

## Appointment / Visit Lifecycle

Known business states include `Scheduled`, `Cancelled`, `No-show`, and `Completed`. Completed work may carry notes, photos, and payment state.

The database layer preserves required distinctions but does not redefine transition rules.

## Payment Lifecycle

A completed visit may be paid or remain outstanding (`BR-034`–`BR-038`). Exact persisted states and transitions remain subject to implementation verification.

## Workspace Media

Visit photos and other user-specific workspace media must remain isolated and preserved across logout and access expiration. Exact file deletion, orphan cleanup, and retention behavior remain open until implementation is verified.

## Open Lifecycle Questions

- permanent client deletion;
- service deactivation or deletion;
- exact payment transitions;
- import/export conflict behavior;
- visit-photo deletion and orphan cleanup;
- backup and restore behavior.

Visible gaps are preferable to invented rules.

## Related Documentation

- [`data-ownership.md`](data-ownership.md)
- [`../../business/requirements/business-rules.md`](../../business/requirements/business-rules.md)
- [`../../business/processes/`](../../business/processes/)
