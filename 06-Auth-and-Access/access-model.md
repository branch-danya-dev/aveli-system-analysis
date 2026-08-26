# Aveli — Access Model

## Purpose

This document describes how Aveli determines whether an authenticated user can open the workspace.

Authentication answers **who the user is**.

Access logic answers **whether that user may currently use the workspace**.

---

## Access Sources

Aveli supports several possible access sources.

Priority order:

```text
Lifetime
   ↓
Manual Grant
   ↓
Active Subscription
   ↓
Active Trial
   ↓
None

The first valid source becomes the effective access source.

Access Decision

Conceptually:

HAS_ACCESS =
    lifetime
    OR manual
    OR activeSubscription
    OR activeTrial

If the result is true, the workspace can be opened.

If the result is false, the user is redirected to the Access Gate.

Access Source Model
Source	Meaning
Lifetime	Permanent access granted manually
Manual	Temporary server-side grant
Subscription	Active store-backed entitlement
Trial	Active 30-day server trial
None	No valid access source

All valid sources unlock the same workspace.

There are no feature-specific premium tiers in the current model.

Priority Rule

If several sources are active at the same time, Aveli resolves one effective source.

Example:

Trial = active
Subscription = active

Effective source:
Subscription

Another example:

Subscription = active
Lifetime = active

Effective source:
Lifetime

Priority keeps access presentation and behavior deterministic.

Trial

Trial access is created once when the account is registered.

Register
   ↓
Create server trial
   ↓
Trial Active
   ↓
Workspace Available

The backend owns trial state.

Therefore trial must not reset because of:

logout;
application reinstall;
local database deletion;
device-side preference reset.
Subscription

Store subscriptions grant the logical entitlement:

support

Both monthly and yearly products map to the same entitlement.

The flow is:

Store Purchase
     ↓
RevenueCat
     ↓
Subscription State
     ↓
Backend Sync
     ↓
Access Resolution

A successful payment flow is not treated as the final access decision by itself.

The entitlement must be resolved into the current access state.

Manual Access

Manual access allows an administrator to grant temporary access independently from store billing.

Conceptually:

Account
   ↓
Manual Grant
   ↓
Valid Until
   ↓
Workspace Access

This source has higher priority than subscription and trial.

Lifetime Access

Lifetime is the highest-priority source.

Lifetime = valid
       ↓
Workspace Access

Other subscription or trial states do not affect access while lifetime access is valid.

No Access

When no valid source exists:

Lifetime       false
Manual         false
Subscription   false
Trial          false
                  ↓
             No Access
                  ↓
             Access Gate

The local workspace remains stored on the device.

Only its availability is blocked.

Access Gate

Access Gate is the single boundary between account state and workspace state.

Authenticated User
        ↓
Access Decision
       / \
      /   \
 Allowed  Denied
   ↓        ↓
Workspace  Access Gate

Individual workspace screens do not independently decide whether they are premium-enabled.

This prevents inconsistent entitlement behavior across the application.

Access State on the Client

The client maintains a resolved access snapshot containing enough information to present the current access state.

Conceptually:

Access State
├── hasAccess
├── source
├── validUntil
├── verification state
└── next verification requirement

The backend remains the trusted authority for server-controlled entitlement state.

Offline Grace

A persisted access snapshot allows temporary offline use.

Last verified access
        ↓
Persist secure snapshot
        ↓
Network unavailable
        ↓
Snapshot still trusted?
       / \
     Yes  No
      ↓    ↓
Workspace Server verification required

The current policy uses a limited offline grace period rather than unlimited cached access.

Refresh Triggers

Access state may need to be refreshed after:

application startup;
session restoration;
application resume;
subscription purchase;
purchase restore;
billing synchronization;
expiration of verification window.

This prevents the client from relying indefinitely on stale entitlement state.

Access and Data Ownership

Access controls availability, not ownership.

Access expires
      ↓
Workspace blocked

NOT

Access expires
      ↓
Delete workspace

Clients, appointments, payments, notes and photos remain stored locally.

If access later becomes valid again, the same workspace can be reopened.

State Summary

The effective high-level access states are:

UNAUTHENTICATED
      ↓
AUTHENTICATED
      ↓
ACCESS CHECK
   /       \
GRANTED   DENIED
   ↓         ↓
WORKSPACE ACCESS GATE

Within GRANTED, the effective source may be:

Lifetime
Manual
Subscription
Trial
Summary

Aveli access logic follows three main principles:

One deterministic access decision
One workspace-level access boundary
Access expiration never destroys operational data

This keeps billing, trial and manual grants separate from the user's actual workspace ownership.