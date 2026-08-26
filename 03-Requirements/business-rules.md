# Aveli — Business Rules

## Purpose

This document defines business rules that control Aveli behavior.

Unlike functional requirements, these rules describe constraints and decisions that must remain true across different user flows.

---

## Access Rules

| ID | Rule |
|---|---|
| BR-001 | A user may access the workspace only when at least one valid access source exists. |
| BR-002 | Access sources are evaluated in the following priority: Lifetime → Manual Grant → Active Subscription → Active Trial → None. |
| BR-003 | Lifetime access has priority over all other access sources. |
| BR-004 | Manual access grant has priority over subscription and trial. |
| BR-005 | An active subscription has priority over trial access. |
| BR-006 | If no valid access source exists, the workspace must be blocked through the Access Gate. |
| BR-007 | Workspace access is granted or denied as a whole; individual workspace features are not separately paywalled. |

---

## Trial Rules

| ID | Rule |
|---|---|
| BR-008 | A 30-day trial is created once when a new server account is registered. |
| BR-009 | Trial state is owned by the backend and must not rely on local storage as the source of truth. |
| BR-010 | Logout must not reset the user's trial. |
| BR-011 | Reinstalling the application must not reset the trial for the same account. |
| BR-012 | Clearing the local workspace database must not create a new trial. |
| BR-013 | Store-managed free trial must not be added on top of the Aveli server trial in the current product model. |

---

## Subscription Rules

| ID | Rule |
|---|---|
| BR-014 | Monthly and yearly products grant the same logical entitlement: `support`. |
| BR-015 | Subscription prices displayed in the application must come from the store / RevenueCat rather than from hardcoded values. |
| BR-016 | A subscription must be treated as auto-renewing where the store product is configured as auto-renewing. |
| BR-017 | The user must be informed that subscription management and cancellation are handled through Google Play or the App Store. |
| BR-018 | A successful purchase does not grant workspace access until the entitlement state is resolved and synchronized. |
| BR-019 | Restoring an existing valid store purchase must restore the corresponding workspace access. |

---

## Data Ownership Rules

| ID | Rule |
|---|---|
| BR-020 | Client, appointment, service, payment, note and visit photo data belong to the local workspace domain. |
| BR-021 | Operational workspace data must not be synchronized to the Aveli backend in the current architecture. |
| BR-022 | Server-side storage is limited to account, session, access and subscription-related data. |
| BR-023 | Each authenticated user must use a separate local workspace database. |
| BR-024 | Visit photos must be isolated by user. |
| BR-025 | Logout must not delete the user's local workspace database or visit photos. |
| BR-026 | Access expiration must not delete or modify existing workspace data. |

---

## Appointment Rules

| ID | Rule |
|---|---|
| BR-027 | An appointment must be associated with a valid client. |
| BR-028 | An appointment must reference a valid service where service selection is required by the workflow. |
| BR-029 | Appointment date and time must satisfy the configured scheduling constraints. |
| BR-030 | Conflicting appointments must be rejected according to the current slot availability rules. |
| BR-031 | A cancelled appointment must not be treated as an active scheduled visit. |
| BR-032 | A no-show appointment must remain distinguishable from a cancelled or completed visit. |
| BR-033 | A completed visit may contain notes, photos and payment information. |

---

## Payment Rules

| ID | Rule |
|---|---|
| BR-034 | Payment may be recorded for a completed visit. |
| BR-035 | A completed visit without received payment may remain in an outstanding state. |
| BR-036 | Outstanding payments must remain visible until resolved. |
| BR-037 | Payment status must not contradict the underlying visit state. |
| BR-038 | Financial summaries must be calculated from locally stored payment data. |

---

## Client Rules

| ID | Rule |
|---|---|
| BR-039 | Client records belong to the active local user workspace. |
| BR-040 | Archived clients remain part of historical data unless explicitly deleted. |
| BR-041 | Client import from device contacts must create Aveli-local client data rather than modifying the source device contact. |
| BR-042 | Client history must be derived from locally stored visits and appointments. |

---

## Session and Logout Rules

| ID | Rule |
|---|---|
| BR-043 | Logout must clear active authentication tokens. |
| BR-044 | Logout must close the currently active user database. |
| BR-045 | Logout must cancel user-specific local reminders. |
| BR-046 | Logout must not remove the user's persistent local workspace data. |
| BR-047 | After another account signs in, data from the previous user must not be exposed in the active workspace. |

---

## Offline Access Rules

| ID | Rule |
|---|---|
| BR-048 | A valid persisted access snapshot may temporarily authorize workspace access without backend connectivity. |
| BR-049 | Cached access is valid only within the configured offline grace period. |
| BR-050 | After the offline grace period expires, server verification is required before continued workspace access. |
| BR-051 | Local workspace operations do not require continuous backend connectivity. |
| BR-052 | Authentication, billing synchronization and entitlement refresh require backend connectivity. |

---

## Reminder Rules

| ID | Rule |
|---|---|
| BR-053 | Appointment reminders are scheduled locally on the device. |
| BR-054 | Reminders must not expose another user's appointment data after account switching. |
| BR-055 | Logout must cancel reminders belonging to the outgoing account. |
| BR-056 | A notification opened by the user should navigate to the related appointment when the reference is still valid. |

---

## Release Rules

| ID | Rule |
|---|---|
| BR-057 | Release builds must use an HTTPS backend endpoint. |
| BR-058 | Release builds must not use emulator or loopback API addresses. |
| BR-059 | Standalone development mode must not be enabled in release builds. |
| BR-060 | RevenueCat secret credentials must never be included in the mobile application. |
| BR-061 | Release configuration must pass automated validation before shipping. |

---

## Core Rule Set

The most important business constraints can be summarized as:

```text
Access is server-controlled
        +
Work data stays local
        +
Subscription does not own user data
        +
Loss of access does not destroy workspace data
        +
Offline work is allowed within verified access policy

Summary

Aveli business rules are centered around three principles:

Access and ownership are separate — billing controls access, not ownership of workspace data.
Operational data is local — the backend does not become the user's working database.
Access decisions are deterministic — trial, subscription and grants follow one consistent priority model.