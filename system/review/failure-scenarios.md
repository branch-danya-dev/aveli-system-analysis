# Aveli — Cross-System Failure Scenarios

> Consolidated non-happy-path scenarios preserved from the legacy offline/error documentation before removal of the numbered architecture.

## Purpose

This document keeps **cross-system failure behavior** that spans authentication, access, billing, local persistence, account switching, reminders, and external services.

It is not the canonical owner of component-specific implementation details or domain rules.

Detailed behavior remains in:

- [`../../frontend/`](../../frontend/)
- [`../../backend/`](../../backend/)
- [`../../database/`](../../database/)
- [`../../integrations/`](../../integrations/)
- [`../../business/requirements/`](../../business/requirements/)

The purpose of this file is to make expected **failure isolation and recovery** visible at the system level.

---

## Failure Principle

Across all scenarios:

```text
Technical Failure
      ≠
Delete User Work
```

Authentication, access, billing, backend, or external-service failures must remain isolated from locally owned professional workspace data whenever possible.

---

## FS-001 — Invalid Credentials

```text
Sign in
   ↓
Credentials invalid
   ↓
Authentication rejected
   ↓
No session created
```

Expected result:

- the unidentified user does not receive an authenticated session;
- no local workspace is opened on behalf of that unidentified identity;
- existing local workspace files are not deleted.

Canonical owners:

- [`../../frontend/auth/`](../../frontend/auth/)
- [`../../backend/auth/`](../../backend/auth/)

---

## FS-002 — Access Token Expired

```text
Authenticated request
      ↓
Access token expired
      ↓
Refresh session
     /       \
 success     failure
   ↓            ↓
continue    re-authenticate
```

Expected result:

- successful refresh continues the session;
- failed refresh requires authentication again;
- local workspace data remain unchanged.

---

## FS-003 — Missing or Corrupted Client Session State

If secure client state is incomplete or cannot be trusted:

```text
Restore session
      ↓
Session state invalid
      ↓
Do not trust identity
      ↓
Return to authentication
```

The client must not solve an authentication-state problem by deleting the user's local database.

---

## FS-004 — Reinstall During Active Trial

```text
Trial active
   ↓
Application reinstalled
   ↓
Sign in to same account
   ↓
Backend returns account-owned trial state
```

Expected result:

- reinstall does not create a new trial;
- local application state is not authoritative for trial creation.

---

## FS-005 — Local Workspace Deleted During Trial

```text
Local workspace deleted
        ≠
Backend trial reset
```

Deleting local professional data does not reset the account-owned trial period.

Canonical access/trial behavior:

[`../../backend/access/`](../../backend/access/)

---

## FS-006 — Trial or Access Changes While App Is Open

If the effective access state changes while the user is already inside the workspace, the application must re-evaluate access according to the current verification policy.

Expected result:

```text
Access state changes
      ↓
Verification occurs when required
      ↓
Access Gate may change
```

but:

```text
Access change
      ≠
Workspace reset
```

---

## FS-007 — Multiple Access Sources Are Active

Example:

```text
Trial        = active
Subscription = active
Lifetime     = active
```

The backend resolves one effective access result according to the canonical precedence.

The frontend must consume the resolved result rather than independently combining access sources.

Canonical decision:

[`../../backend/access/access-resolution.md`](../../backend/access/access-resolution.md)

---

## FS-008 — One Access Source Expires but Another Remains Valid

Example:

```text
Subscription = expired
Manual Grant = valid
```

Expected result:

- workspace access remains available;
- the effective source changes to the still-valid higher-priority/remaining source according to backend resolution;
- no local workspace mutation occurs because one access source expired.

---

## FS-009 — No Access but Local Data Exists

```text
No valid access
      ↓
Access Gate
```

Existing:

```text
SQLite workspace
visit photos
professional history
```

remain preserved.

This is an access-state condition, not a data-deletion event.

---

## FS-010 — App Starts Offline With Valid Trusted Snapshot

```text
No network
   ↓
Trusted snapshot available
   ↓
Verification policy accepts it
   ↓
Workspace opens
```

The user may continue local work while the cached access state remains trusted by the current policy.

Canonical offline behavior:

[`../../frontend/offline/`](../../frontend/offline/)

---

## FS-011 — App Starts Offline Without Trusted Snapshot

```text
No network
   ↓
No trusted snapshot
   ↓
Access cannot be verified
   ↓
Network verification required
```

The system must not guess that access is valid.

Local professional data remain preserved.

---

## FS-012 — Offline Verification Window Expires

```text
Cached access previously valid
        ↓
Verification deadline exhausted
        ↓
Backend verification required
```

The application must not convert temporary cached trust into permanent offline authorization.

Expected result:

- workspace authorization may become unavailable;
- local workspace data remain stored.

---

## FS-013 — Purchase Succeeds but Backend Reconciliation Fails

```text
Store purchase succeeds
        ↓
RevenueCat state updated
        ↓
POST /v1/billing/sync fails
```

Expected result:

- the purchase is not treated as if it never occurred;
- local workspace data remain unchanged;
- billing reconciliation can be retried;
- Aveli workspace access is refreshed only after backend reconciliation succeeds or another valid access source applies.

This preserves the boundary:

```text
Provider purchase result
        ≠
Aveli workspace access
```

Canonical billing flow:

- [`../../frontend/billing/`](../../frontend/billing/)
- [`../../backend/billing/`](../../backend/billing/)
- [`../../integrations/revenuecat/`](../../integrations/revenuecat/)

---

## FS-014 — Backend Reconciliation Succeeds but Client State Is Stale

The backend may already hold a valid normalized subscription while the frontend still presents stale access state.

Expected recovery:

```text
Backend state valid
      ↓
Client refreshes AccessState
      ↓
Current access result restored
```

The client must be able to recover without recreating account or workspace data.

---

## FS-015 — Restore Finds No Active Subscription

```text
Restore purchase
      ↓
No active support entitlement
      ↓
No subscription access
```

This is a valid business outcome, not necessarily a technical failure.

Other access sources may still independently provide access.

---

## FS-016 — RevenueCat Webhook Is Late or Repeated

Mobile billing synchronization and webhook delivery may occur at different times.

Expected backend behavior:

```text
Webhook received
      ↓
Idempotency check
      ↓
Provider reconciliation
      ↓
Normalized subscription state
```

Repeated or delayed webhook processing must not create contradictory access state.

Canonical webhook behavior:

[`../../integrations/revenuecat/webhooks.md`](../../integrations/revenuecat/webhooks.md)

---

## FS-017 — Logout With Existing Professional Data

Logout must terminate active identity/session context without deleting the workspace.

Expected cross-system behavior:

```text
Logout
  ↓
Clear active client session/access state
  ↓
Close current user database
  ↓
Cancel account-specific reminders
```

Preserved:

```text
local database
visit photos
professional history
```

Canonical lifecycle:

[`../../system/flows/logout-and-profile-delete.md`](../flows/logout-and-profile-delete.md)

---

## FS-018 — Different User Signs In

```text
User A logout
      ↓
User B login
      ↓
Open User B local workspace
```

Expected result:

- User A records do not appear in User B workspace;
- local database and media namespaces remain isolated by authenticated user identity.

---

## FS-019 — Local Database Migration Fails

A local migration failure is a data-integrity problem.

The system should not silently recover by recreating an empty database if doing so would destroy recoverable workspace data.

Expected principle:

```text
Migration failure
      ↓
Surface / isolate persistence failure
      ↓
Preserve recoverable data
```

Canonical migration ownership:

[`../../database/local/migrations/`](../../database/local/migrations/)

---

## FS-020 — Visit Photo Metadata Exists but File Is Missing

```text
Photo metadata exists
        +
Physical file missing
        ↓
Media item unavailable
```

Expected result:

- the missing media item is handled as unavailable;
- the entire visit or appointment is not treated as corrupted solely because one file is missing.

---

## FS-021 — Reminder Refers to a Missing Appointment

A notification payload may reference an appointment that no longer exists.

Expected result:

```text
Notification tap
      ↓
Appointment not found
      ↓
Safe navigation fallback
```

The application should not fail navigation catastrophically because the referenced local entity was deleted or changed.

---

## FS-022 — Logout Before Reminder Fires

Account-specific reminders must be cancelled during logout.

This prevents appointment information from a previous account from appearing after another account becomes active.

---

## FS-023 — External Exchange-Rate Service Is Unavailable

```text
Rate refresh fails
      ↓
Currency conversion unavailable / fallback
```

Expected result:

- unrelated workspace operations remain available;
- existing local professional data remain unchanged.

Canonical integration:

[`../../integrations/exchange-rate/`](../../integrations/exchange-rate/)

---

## FS-024 — Store Service Is Unavailable

If Apple App Store or Google Play cannot complete a new purchase/restore action:

- current access state does not change merely because the store is unavailable;
- the user may remain on the Access Gate if no other access source exists;
- local workspace data remain unaffected.

---

## Domain-Specific Failures

The legacy document also included appointment, client, service, and payment domain edge cases.

They are intentionally **not made canonical here**.

Examples include:

```text
appointment conflict
appointment state transition
referenced client deletion
duplicate payment action
invalid payment state
```

Their final rules belong in business/domain requirements and owning frontend behavior, not in a system-level failure catalog.

During final polish, unresolved domain cases should be reconciled against:

[`../../business/requirements/`](../../business/requirements/)

---

## Summary

The main cross-system failure categories are:

```text
authentication recovery
access/trial transitions
offline verification
billing reconciliation
account/workspace isolation
local persistence integrity
reminder recovery
external service failure
```

The common expectation is:

```text
deterministic behavior
+
recoverable state
+
failure isolation
+
protection of locally owned work
```
