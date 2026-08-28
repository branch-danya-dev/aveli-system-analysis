# Aveli — Access Journey

<p align="center">
  <a href="access-journey.md"><b>English</b></a> ·
  <a href="access-journey.ru.md">Русский</a>
</p>

> Describes the product-level journey used to determine whether the current user may open the Aveli workspace.

---

## Purpose

Access is a workspace-level product decision.

The journey answers one question:

> **May the current authenticated user open the workspace now?**

Aveli supports several access sources, but all valid sources unlock the same professional workspace.

This document describes the business flow only.

Technical access resolution belongs to:

```text
../../backend/access/
../../frontend/bootstrap/
../../frontend/offline/
```

---

## Access Sources

The current access priority is:

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
```

If more than one source is valid, the highest-priority valid source represents the current access state.

Conceptually:

```text
HAS_ACCESS =
    lifetime
    OR manual
    OR activeSubscription
    OR activeTrial
```

A valid access source unlocks the complete workspace.

There are no independent premium feature checks in the current product model.

---

## New User Access Journey

After a new user creates an account:

```text
Create account
    ↓
Trial becomes active
    ↓
Access becomes valid
    ↓
Workspace opens
```

The trial belongs to the account.

It is not restarted by:

- logout;
- reinstall;
- clearing local workspace data.

Related rules:

`BR-008`–`BR-013`

---

## Returning User Access Journey

For an existing user:

```text
Launch Aveli
    ↓
Authenticated state available?
   /                       \
 Yes                       No
  ↓                         ↓
Evaluate access          Sign in
  ↓                         ↓
Valid access?           Evaluate access
   /       \                 ↓
 Yes       No            Continue below
  ↓         ↓
Workspace  Access Gate
```

The workspace is not shown until an access decision exists.

---

## Access Decision

The decision process is:

```text
Evaluate valid access sources
    ↓
Lifetime valid?
   /          \
 Yes          No
  ↓            ↓
Allow       Manual grant valid?
              /          \
            Yes          No
             ↓            ↓
           Allow       Subscription valid?
                         /            \
                       Yes            No
                        ↓              ↓
                      Allow         Trial valid?
                                     /       \
                                   Yes       No
                                    ↓         ↓
                                  Allow      Deny
```

The priority determines which access source is considered active when several are valid.

It does not create different workspace capability levels.

---

## Trial Expiration Journey

When the trial reaches its end:

```text
Trial expires
    ↓
Evaluate access
    ↓
Another valid source exists?
   /                         \
 Yes                         No
  ↓                           ↓
Workspace remains available  Access Gate
```

Trial expiration does not delete professional workspace information.

If access later becomes valid again, the same user workspace becomes available again.

---

## Subscription Purchase Journey

When the user has no valid access source, they may obtain subscription-based access.

At product level:

```text
Access Gate
    ↓
Open subscription option
    ↓
Select supported plan
    ↓
Complete platform purchase flow
    ↓
Subscription state is reconciled
    ↓
Evaluate access
    ↓
Valid subscription?
   /                \
 Yes                No
  ↓                  ↓
Workspace            Access Gate
```

Monthly and yearly plans provide the same logical workspace access.

Provider-specific purchase and reconciliation details belong to:

`../../integrations/`

---

## Restore Purchase Journey

A user may restore an existing purchase.

```text
Request restore
    ↓
Existing subscription state is recovered
    ↓
Subscription state is reconciled
    ↓
Evaluate access
    ↓
Valid subscription?
   /                \
 Yes                No
  ↓                  ↓
Workspace            Access Gate
```

A successful restore results in workspace access only when a valid subscription-based access state exists.

---

## Manual and Lifetime Access

Aveli may also provide access through non-subscription sources.

### Manual Grant

A valid manual grant provides workspace access and has priority over subscription and trial.

### Lifetime Access

Lifetime access provides workspace access without an expiration-based dependency on trial or subscription.

It has the highest priority in the current access model.

The mechanism used to create or administer these grants is outside this business process.

---

## Offline Access Journey

A previously verified access state may temporarily support workspace use without connectivity.

Product-level flow:

```text
Connectivity unavailable
    ↓
Previously verified access available?
   /                             \
 Yes                             No
  ↓                               ↓
Verification still sufficient?  Access Gate
   /                  \
 Yes                  No
  ↓                    ↓
Workspace          Verification required
```

The offline period is limited.

When the existing verification is no longer considered sufficient, renewed verification is required.

The exact verification mechanism and duration belong to the technical access documentation.

---

## Access Gate

The Access Gate is the single product boundary between denied access and the professional workspace.

```text
Authenticated user
    ↓
Access Decision
   /             \
Allowed          Denied
  ↓                ↓
Workspace       Access Gate
```

This model prevents the product from entering partially unlocked states where unrelated screens independently decide whether the user has access.

The Access Gate may provide routes to supported access-restoration actions such as subscription purchase or purchase restoration.

---

## Data Preservation

Loss of access does not remove professional workspace information.

```text
Access expires
    ↓
Workspace becomes unavailable
    ↓
Professional information remains preserved
    ↓
Access becomes valid again
    ↓
Workspace becomes available again
```

Access controls availability.

It does not redefine ownership of existing professional workspace information.

---

## Access Journey Summary

Aveli separates three product concepts:

```text
Identity
    ↓
Access
    ↓
Workspace
```

Authentication establishes who the user is.

Access determines whether that user may currently enter the workspace.

The workspace contains the user's professional activity and remains conceptually separate from the access lifecycle.

---

## Related Documentation

- [`main-user-journey.md`](main-user-journey.md)
- [`../requirements/business-rules.md`](../requirements/business-rules.md)
- [`../requirements/functional-requirements.md`](../requirements/functional-requirements.md)
- [`../requirements/acceptance-criteria.md`](../requirements/acceptance-criteria.md)
- [`../../backend/access/`](../../backend/access/)
- [`../../frontend/offline/`](../../frontend/offline/)
- [`../../integrations/`](../../integrations/)
