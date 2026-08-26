# Aveli — Offline Behavior

## Purpose

This document describes how Aveli behaves when network connectivity is unavailable or unstable.

Aveli is designed so that daily workspace operations do not depend on continuous backend availability.

---

## Offline Principle

Aveli separates:

```text
Local Work
    ↓
Available offline

Account / Access / Billing
    ↓
Requires backend periodically

The workspace remains local to the device, while account-related state is verified through the backend when required.

What Works Offline

The following operations can continue without network access:

viewing clients;
creating and editing clients;
viewing appointments;
creating and rescheduling appointments;
managing services;
recording payments;
viewing outstanding payments;
saving visit notes;
saving visit photos;
using local reminders;
browsing locally stored workspace history.

These operations use the local SQLite database and local file storage.

What Requires Connectivity

The following operations require backend or external service access:

registration;
sign-in when no valid session is available;
access verification after the trusted window expires;
subscription purchase synchronization;
purchase restore synchronization;
entitlement refresh;
RevenueCat-related state reconciliation;
exchange-rate updates.
Cached Access State

The client stores a trusted access snapshot in secure storage.

Conceptually:

Verified Access
      ↓
Persist Snapshot
      ↓
Network unavailable
      ↓
Evaluate snapshot age

The cached snapshot is not permanent authorization.

It is only trusted according to the current offline verification policy.

Offline Grace

The current access policy supports a limited offline grace period.

Backend unavailable
      ↓
Cached snapshot exists?
     / \
   Yes  No
    ↓    ↓
Check   Verification
grace   required
    ↓
Still valid?
   / \
 Yes  No
  ↓    ↓
Workspace Verification required

The current concept uses approximately 72 hours of offline grace.

Startup Without Network

When the application starts offline:

Launch
   ↓
Restore local session
   ↓
Load access snapshot
   ↓
Evaluate verification policy
   ↓
Snapshot trusted?
  /           \
Yes           No
 ↓             ↓
Open        Access verification
Workspace   required

The local workspace database remains untouched regardless of the result.

Working Offline

Once workspace access is allowed, normal operations continue against local storage.

User Action
    ↓
Domain Rule
    ↓
Local Repository
    ↓
SQLite / Local Files

No backend synchronization is required for normal workspace entities.

Network Restoration

When connectivity returns:

Network restored
      ↓
Verification required?
     / \
   Yes  No
    ↓    ↓
Refresh Continue
access  local work

Aveli may refresh:

authentication session;
RevenueCat customer state;
billing state;
backend access state.

Local work data is not uploaded as part of this process.

Subscription During Offline Mode

A subscription cannot be fully reconciled while required external services are unavailable.

Example:

Access Gate
    ↓
No network
    ↓
Purchase / restore unavailable

Existing local data remains preserved.

Once connectivity returns, billing state can be synchronized and access recalculated.

Backend Failure

Backend failure must not be treated as workspace data failure.

Backend unavailable
      ↓
Access verification fails
      ↓
Preserve local database
      ↓
Use cached access if allowed

The system must never respond to backend unavailability by deleting or resetting operational data.

Expired Offline Grace

If the cached access state is no longer trusted:

Offline grace expired
       ↓
Server verification required
       ↓
Workspace cannot be re-authorized offline

This is an access-state problem, not a data-loss event.

The workspace remains stored locally.

Account Switching Offline

Account switching is limited by authentication availability.

If another user cannot establish a valid authenticated session, the application must not open that user's workspace based on another account's cached state.

Local user databases remain isolated.

Reminder Behavior

Appointment reminders are local and do not require server connectivity after being scheduled.

Appointment
    ↓
Schedule local reminder
    ↓
Device notification service

This keeps reminders independent from backend availability.

Failure Isolation

The architecture separates failure domains:

Failure	Expected Impact
Backend unavailable	Access refresh may fail
RevenueCat unavailable	Purchase / restore may fail
Store unavailable	New subscription action unavailable
Exchange-rate API unavailable	Rate refresh unavailable
Local DB available	Workspace continues locally
Local DB unavailable/corrupted	Workspace-specific failure
Key Constraint

Offline operation does not mean unlimited access.

The intended model is:

Offline-capable workspace
        +
Time-limited trusted entitlement

This allows Aveli to remain usable during temporary connectivity problems without turning cached entitlement into permanent authorization.

Summary

Aveli offline behavior is based on three rules:

Daily workspace operations are local.
Access can temporarily rely on a trusted cached snapshot.
Connectivity failures must never destroy operational data.