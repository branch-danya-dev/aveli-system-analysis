# Aveli — Edge Cases

## Purpose

This document describes important non-happy-path scenarios that can affect authentication, access, billing, local data and daily workspace operations.

The goal is to make expected system behavior explicit before implementation or testing.

---

## Authentication Edge Cases

### Invalid Credentials

```text
Sign in
   ↓
Credentials invalid
   ↓
Reject authentication
   ↓
No session created

The local workspace must not be opened for an unidentified user.

Expired Access Token
Authenticated request
      ↓
Access token expired
      ↓
Use refresh token

If refresh succeeds, the session continues.

If refresh fails, re-authentication is required.

Local workspace data remains unchanged.

Missing or Corrupted Token State

If secure token storage contains incomplete or invalid session information:

Restore session
     ↓
Session cannot be trusted
     ↓
Return to authentication

The system must not delete the user's local database.

Trial Edge Cases
Reinstall During Trial
Trial active
   ↓
Reinstall application
   ↓
Sign in to same account
   ↓
Backend returns existing trial state

A new trial must not be created.

Local Database Deleted During Trial

Deleting local work data does not affect server-side trial state.

Local DB deleted
      ≠
Trial reset

The account continues using the original trial period.

Trial Expires While App Is Open

If trial validity changes while the user is already inside the workspace, the system must refresh access when the current verification policy requires it.

The workspace must not be deleted or reset.

Access Edge Cases
Multiple Access Sources Active

Example:

Trial = active
Subscription = active
Lifetime = active

The effective source is resolved by priority:

Lifetime

Only one effective source should be presented to access logic.

Subscription Expires but Manual Grant Exists
Subscription = expired
Manual Grant = valid

Workspace access remains available.

The effective source becomes Manual.

No Access but Local Data Exists
No valid entitlement
       ↓
Access Gate

Existing SQLite data and visit photos remain untouched.

Offline Edge Cases
App Starts Offline With Valid Snapshot
No network
   ↓
Snapshot valid
   ↓
Workspace opens

The user can continue local work within the configured offline grace period.

App Starts Offline Without Snapshot
No network
   ↓
No trusted snapshot
   ↓
Access cannot be verified

The system must not guess that access is valid.

Local data remains preserved.

Offline Grace Expires During Extended Outage

Once the trusted verification window is exhausted, continued access requires backend verification.

The application must not convert a temporary cached entitlement into permanent offline access.

Billing Edge Cases
Purchase Succeeds but Backend Sync Fails
Store purchase succeeds
      ↓
RevenueCat updated
      ↓
Backend unavailable

Expected behavior:

purchase state is not treated as lost;
local workspace data remains unchanged;
billing reconciliation can be retried later;
access is refreshed once backend synchronization succeeds.
Billing Sync Succeeds but Client Refresh Fails

The backend may already contain the valid subscription while the client still shows stale state.

The client must be able to recover by refreshing AccessState.

Restore Returns No Subscription
Restore
   ↓
No active support entitlement
   ↓
Access remains denied

This is a valid business outcome, not a technical error.

RevenueCat Webhook Arrives Late

The mobile billing sync and webhook path may update subscription state at different times.

Backend processing must remain idempotent enough that repeated subscription updates do not create contradictory entitlement state.

Local Data Edge Cases
User Logs Out With Existing Data

Logout must:

clear active session state;
close current database;
cancel user-specific reminders.

It must not delete local workspace data.

Different User Signs In
User A logout
     ↓
User B login
     ↓
Open User B database

User A clients, appointments, notes or photos must not appear in User B workspace.

Local Database Migration Fails

A migration failure must be treated as a local data integrity problem.

The system should avoid silently recreating an empty database if that would destroy recoverable user data.

Visit Photo File Missing

If photo metadata exists but the physical file is unavailable:

Photo metadata exists
       +
File missing
       ↓
Handle unavailable media

The failure should affect that media item rather than corrupt the entire visit or appointment.

Appointment Edge Cases
Time Conflict

When a new or rescheduled appointment conflicts with existing schedule rules:

Select time
   ↓
Conflict detected
   ↓
Reject invalid slot

The previous valid appointment state must remain unchanged.

Appointment Rescheduled During Edit

If the selected slot becomes invalid before save, the final save operation must revalidate scheduling rules rather than rely only on initial UI availability.

Cancel Completed Appointment

A completed visit should not silently transition into an incompatible appointment state if that would contradict payment or visit history.

Such transitions must follow explicit domain rules.

Delete Referenced Client

If a client has historical appointments, deletion behavior must preserve domain consistency.

Where full deletion is not allowed, archive behavior should be used instead.

Payment Edge Cases
Complete Visit Without Payment

The visit may be completed while payment remains outstanding.

Visit completed
      ↓
No payment
      ↓
Outstanding
Duplicate Payment Action

Repeated user actions must not create contradictory or duplicate payment state for the same completed visit.

Payment Recorded for Invalid Visit State

The system must reject payment operations that violate the current visit/payment business rules.

Reminder Edge Cases
Logout Before Reminder Fires

User-specific reminders must be cancelled on logout.

This prevents appointment information from one account appearing after another account signs in.

Appointment Deleted or Rescheduled

Scheduled reminders must be updated or removed so they do not reference stale appointment data.

Notification Opens Missing Appointment

If a notification references an appointment that no longer exists, the application should fall back safely rather than fail navigation.

External Service Edge Cases
Exchange Rate API Unavailable

Failure to refresh exchange rates must not block unrelated workspace operations.

Existing local values remain usable according to the product's currency rules.

Store Service Unavailable

If Google Play or App Store cannot complete a purchase:

the current access state remains unchanged;
the user stays on the Access Gate if no other access source exists;
local data is unaffected.
Release Edge Cases
Production Build Uses Emulator API

Release validation must reject configurations such as:

10.0.2.2
localhost
127.0.0.1
Standalone Mode Enabled in Release

A production build must fail release validation if development-only standalone behavior is enabled.

Missing RevenueCat Configuration

The application must fail configuration validation or disable the affected billing flow explicitly rather than shipping with silently broken subscription behavior.

Failure Principle

Across all edge cases, one rule remains constant:

Technical Failure
      ≠
Delete User Work

Authentication, billing, access or external integration failures must remain isolated from locally owned workspace data whenever possible.

Summary

The most important edge cases in Aveli fall into five categories:

authentication and session recovery;
access and trial transitions;
billing synchronization;
local data integrity;
offline and external service failures.

The expected behavior is always deterministic, recoverable and protective of locally stored work data.