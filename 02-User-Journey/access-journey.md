# Aveli — Access Journey

## Purpose

This document describes how Aveli determines whether the user can enter the workspace.

Access is controlled centrally and depends on the current entitlement state.

---

## Access Sources

Aveli supports several sources of access.

Priority:

```text
Lifetime
   ↓
Manual Grant
   ↓
Active Subscription
   ↓
Active Trial
   ↓
No Access

A valid source grants access to the complete workspace.

New User

After registration:

Register
   ↓
Backend creates 30-day trial
   ↓
Access state = Trial
   ↓
Workspace opens

The trial is created once on the server.

It is not reset by:

logout;
reinstall;
clearing local application data.
Returning User

On application startup:

Restore session
    ↓
Load cached access snapshot
    ↓
Verify access if required
    ↓
Resolve current access source
    ↓
Access granted?
   /          \
 Yes          No
  ↓            ↓
Workspace    Access Gate

The workspace is not shown before the access decision is made.

Access Decision

Conceptually:

HAS_ACCESS =
    lifetime
    OR manual
    OR activeSubscription
    OR activeTrial

If several access sources are valid, the highest-priority source is used as the current access source.

Trial Journey
Registration
    ↓
Trial created on backend
    ↓
30 days of access
    ↓
Trial expires
    ↓
Access verification
    ↓
No other valid source?
   /             \
 Yes             No
  ↓               ↓
Access Gate     Workspace

The local workspace data remains on the device after trial expiration.

Subscription Journey

When access is unavailable, the user may purchase support through the store.

Access Gate
    ↓
Open support paywall
    ↓
Select monthly / yearly plan
    ↓
Store purchase
    ↓
RevenueCat entitlement updated
    ↓
Billing sync with backend
    ↓
Access refreshed
    ↓
Workspace opens

Both subscription plans provide the same logical entitlement: support.

Restore Purchase

A user may restore an existing store purchase.

Restore purchase
    ↓
RevenueCat retrieves customer state
    ↓
Backend billing sync
    ↓
Access state refreshed
    ↓
Valid subscription?
   /          \
 Yes          No
  ↓            ↓
Workspace    Access Gate
Offline Access

Aveli stores a trusted access snapshot in secure storage.

If the backend is temporarily unavailable:

No network
    ↓
Cached access snapshot exists?
    ↓
Snapshot within offline grace?
   /                    \
 Yes                    No
  ↓                      ↓
Workspace            Access Gate

The current policy allows a limited offline grace period before server verification becomes mandatory.

Access Gate

The Access Gate is the single workspace-level access boundary.

Application
    ↓
Access Decision
   /          \
Allowed      Denied
  ↓            ↓
Workspace   Access Gate

Aveli does not distribute premium checks across individual screens.

This prevents inconsistent states where some workspace features remain available while others are blocked.

Local Data Preservation

Loss of access does not delete operational data.

Access expires
      ↓
Workspace blocked
      ↓
SQLite remains
Photos remain
      ↓
Access restored
      ↓
Same workspace data becomes available again
Summary

Aveli separates:

Authentication
      ↓
Entitlement
      ↓
Workspace Access

The backend remains the source of truth for trial and access decisions, while a secure local snapshot supports temporary offline operation.