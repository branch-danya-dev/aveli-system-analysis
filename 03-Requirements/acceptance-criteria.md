# Aveli — Acceptance Criteria

## Purpose

This document defines observable conditions used to verify that Aveli behavior matches the agreed functional requirements and business rules.

Acceptance criteria focus on externally verifiable system behavior rather than implementation details.

---

## Authentication

| ID | Acceptance Criterion |
|---|---|
| AC-001 | A new user can register and receive an authenticated session. |
| AC-002 | An existing user can sign in with valid credentials. |
| AC-003 | Invalid authentication credentials do not create a session. |
| AC-004 | Logout clears the active authenticated session. |
| AC-005 | Logout does not delete the user's local workspace database or visit photos. |

---

## Trial and Access

| ID | Acceptance Criterion |
|---|---|
| AC-006 | A newly registered account receives a 30-day trial from the backend. |
| AC-007 | Logging out and signing in again does not create a new trial. |
| AC-008 | Reinstalling the application does not reset the trial for the same account. |
| AC-009 | A user with at least one valid access source can open the workspace. |
| AC-010 | A user without any valid access source is redirected to the Access Gate. |
| AC-011 | Workspace features are not independently blocked by separate premium checks. |
| AC-012 | Existing local work data remains stored after access expiration. |
| AC-013 | Restoring valid access makes the existing local workspace available again. |

The current product definition explicitly treats trial as server-owned and keeps local work data after access loss. :contentReference[oaicite:0]{index=0} :contentReference[oaicite:1]{index=1}

---

## Subscription

| ID | Acceptance Criterion |
|---|---|
| AC-014 | The user can start a supported monthly or yearly subscription purchase flow. |
| AC-015 | A successful store purchase results in the `support` entitlement being recognized. |
| AC-016 | Billing state can be synchronized with the backend. |
| AC-017 | A valid synchronized subscription grants workspace access. |
| AC-018 | Restoring an existing valid purchase restores access. |
| AC-019 | Subscription prices displayed in the UI are obtained from the store / RevenueCat. |
| AC-020 | The UI clearly indicates that the subscription is recurring and managed through the platform store. |

Both plans map to the same `support` entitlement, while billing sync is handled through the backend integration. :contentReference[oaicite:2]{index=2} :contentReference[oaicite:3]{index=3}

---

## Local Workspace Data

| ID | Acceptance Criterion |
|---|---|
| AC-021 | Client, appointment, service, payment, note and photo data can be used without continuous backend connectivity. |
| AC-022 | Workspace records are stored in the active user's local database. |
| AC-023 | Data from one account is not shown when another account is active. |
| AC-024 | Visit photos are stored in a user-specific location. |
| AC-025 | Logout closes the current workspace without deleting its persistent data. |

Aveli deliberately separates local operational data from server-side account and access data. :contentReference[oaicite:4]{index=4}

---

## Clients

| ID | Acceptance Criterion |
|---|---|
| AC-026 | The user can create a client and see the new client in the local directory. |
| AC-027 | The user can edit an existing client. |
| AC-028 | The user can archive and restore a client. |
| AC-029 | Client details show locally available visit history. |
| AC-030 | Device contacts can be imported into the Aveli client directory where permission is granted. |

---

## Appointments

| ID | Acceptance Criterion |
|---|---|
| AC-031 | The user can create an appointment for a valid client, service, date and time. |
| AC-032 | The created appointment appears in the relevant Today and Calendar views. |
| AC-033 | The system prevents scheduling states that violate configured conflict rules. |
| AC-034 | The user can reschedule an appointment. |
| AC-035 | The user can cancel an appointment. |
| AC-036 | The user can mark an appointment as no-show. |
| AC-037 | The user can complete a visit. |
| AC-038 | A completed visit can store notes and visit photos. |

---

## Payments

| ID | Acceptance Criterion |
|---|---|
| AC-039 | A payment can be recorded for a completed visit. |
| AC-040 | A completed visit without payment can remain outstanding. |
| AC-041 | Outstanding payments appear in the unpaid list. |
| AC-042 | Period financial information is calculated from locally stored payment data. |

---

## Reminders

| ID | Acceptance Criterion |
|---|---|
| AC-043 | A local reminder can be scheduled for an appointment. |
| AC-044 | Opening a valid appointment reminder navigates to the corresponding appointment where possible. |
| AC-045 | Logging out cancels reminders associated with the outgoing user. |
| AC-046 | After account switching, reminders do not expose another user's appointment data. |

The current implementation explicitly uses local notifications and cancels them on logout. :contentReference[oaicite:5]{index=5}

---

## Offline Access

| ID | Acceptance Criterion |
|---|---|
| AC-047 | A valid persisted access snapshot allows temporary workspace access while the backend is unavailable. |
| AC-048 | Workspace access continues only while the offline grace policy considers the snapshot valid. |
| AC-049 | After offline grace expires, backend verification is required before access continues. |
| AC-050 | Temporary backend failure does not delete or corrupt local workspace data. |

Aveli keeps a persisted access snapshot and applies an offline verification policy before opening the workspace. :contentReference[oaicite:6]{index=6}

---

## Release Safety

| ID | Acceptance Criterion |
|---|---|
| AC-051 | A release build fails validation if configured with a loopback or emulator API address. |
| AC-052 | A release build fails validation if standalone development mode is enabled. |
| AC-053 | Production API configuration uses HTTPS. |
| AC-054 | Secret RevenueCat credentials are absent from the mobile application bundle. |
| AC-055 | Required release configuration passes automated checks before shipping. |

These constraints are explicitly part of the current release model. :contentReference[oaicite:7]{index=7}

---

## Definition of Accepted Behavior

The current concept can be considered accepted when:

```text
Register
   ↓
Receive server trial
   ↓
Use full local workspace
   ↓
Trial expires
   ↓
Local data remains intact
   ↓
Purchase / restore access
   ↓
Workspace reopens with the same data

This matches the project's current definition of "ready": server trial, persistent local data, unified access gate, subscription sync, and release configuration safety.

Summary

Acceptance criteria verify four major qualities of Aveli:

Access behaves predictably.
Operational data remains local and persistent.
Daily workflows remain usable and consistent.
Production builds cannot ship with unsafe development configuration.