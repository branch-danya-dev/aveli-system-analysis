# Aveli — Functional Requirements

## Purpose

This document defines the functional behavior required from Aveli.

The requirements focus on observable system behavior without describing implementation details.

---

## Account and Authentication

| ID | Requirement |
|---|---|
| FR-001 | The system must allow a new user to create an account. |
| FR-002 | The system must allow an existing user to sign in. |
| FR-003 | The system must restore an authenticated session when valid session data exists. |
| FR-004 | The system must support logout without deleting the user's local workspace data. |
| FR-005 | The system must associate local workspace data with the authenticated user. |

---

## Access and Trial

| ID | Requirement |
|---|---|
| FR-006 | The backend must create a 30-day trial when a new account is registered. |
| FR-007 | The trial must be associated with the server-side account rather than only with local application state. |
| FR-008 | The system must determine whether the user currently has workspace access. |
| FR-009 | Access may be granted by lifetime access, manual grant, active subscription or active trial. |
| FR-010 | The application must prevent workspace access when no valid access source exists. |
| FR-011 | The workspace must be controlled through a single access boundary rather than independent per-feature checks. |
| FR-012 | The application must preserve local workspace data when access expires. |
| FR-013 | Previously stored workspace data must become available again after access is restored. |

---

## Subscription and Billing

| ID | Requirement |
|---|---|
| FR-014 | The user must be able to purchase access through supported store subscription plans. |
| FR-015 | The user must be able to restore an existing store purchase. |
| FR-016 | Store subscription state must be synchronized with the backend. |
| FR-017 | A valid store subscription must result in a valid workspace access state. |
| FR-018 | Monthly and yearly subscription products must grant the same logical `support` entitlement. |
| FR-019 | The application must display store-localized subscription pricing. |

---

## Clients

| ID | Requirement |
|---|---|
| FR-020 | The user must be able to create a client. |
| FR-021 | The user must be able to update client data. |
| FR-022 | The user must be able to archive and restore clients. |
| FR-023 | The user must be able to delete a client where business rules allow it. |
| FR-024 | The user must be able to search and browse the client directory. |
| FR-025 | The user must be able to view client details and visit history. |
| FR-026 | The user must be able to import clients from device contacts. |

---

## Services

| ID | Requirement |
|---|---|
| FR-027 | The user must be able to create and update services. |
| FR-028 | A service must support pricing and duration data. |
| FR-029 | The user must be able to delete services where allowed by the current data state. |

---

## Appointments

| ID | Requirement |
|---|---|
| FR-030 | The user must be able to create an appointment. |
| FR-031 | An appointment must be associated with a client, service, date and time. |
| FR-032 | The system must prevent invalid appointment time conflicts according to scheduling rules. |
| FR-033 | The user must be able to reschedule an appointment. |
| FR-034 | The user must be able to cancel an appointment. |
| FR-035 | The user must be able to mark an appointment as no-show. |
| FR-036 | The user must be able to complete a visit. |
| FR-037 | The completed visit may contain notes and photos. |
| FR-038 | The system must show appointments in the relevant calendar and daily views. |

---

## Payments

| ID | Requirement |
|---|---|
| FR-039 | The user must be able to record payment for a completed visit. |
| FR-040 | The system must support unpaid or outstanding payment state. |
| FR-041 | The user must be able to view outstanding payments. |
| FR-042 | The system must provide period-based financial information from locally stored payment data. |

---

## Calendar and Today

| ID | Requirement |
|---|---|
| FR-043 | The application must provide a daily workspace overview. |
| FR-044 | The application must provide calendar-based navigation of appointments. |
| FR-045 | The user must be able to navigate between supported calendar dates. |
| FR-046 | The calendar must reflect current appointment state after create, reschedule, cancel or complete operations. |

---

## Reminders

| ID | Requirement |
|---|---|
| FR-047 | The application must support local appointment reminders. |
| FR-048 | Reminders must be scheduled on the user's device. |
| FR-049 | The application must cancel user-specific reminders on logout. |
| FR-050 | Opening the application from a supported reminder must navigate to the related appointment where possible. |

---

## Profile and Settings

| ID | Requirement |
|---|---|
| FR-051 | The user must be able to manage profile information. |
| FR-052 | The user must be able to configure working schedule settings. |
| FR-053 | The user must be able to configure application language. |
| FR-054 | The application must support Russian and English localization. |
| FR-055 | The user must be able to configure supported appearance settings. |
| FR-056 | The user must be able to change the profile currency. |
| FR-057 | The application must support profile data export and import. |

---

## Local Data

| ID | Requirement |
|---|---|
| FR-058 | Operational workspace data must be stored locally on the device. |
| FR-059 | Local workspace data must be isolated by authenticated user. |
| FR-060 | Visit photos must be stored in a user-specific local file location. |
| FR-061 | Logout must not delete the user's local database or visit photos. |
| FR-062 | Access expiration must not delete local workspace data. |

---

## Offline Behavior

| ID | Requirement |
|---|---|
| FR-063 | The application must allow local workspace operations without permanent network connectivity. |
| FR-064 | The application must use a persisted access snapshot to support temporary offline access. |
| FR-065 | Offline workspace access must be limited by the current access verification policy. |
| FR-066 | Operations requiring authentication, entitlement refresh or billing synchronization must require backend connectivity. |

---

## Processing Boundary

Aveli separates two categories of behavior:

```text
Workspace Operations
        ↓
Local Device Data

Account / Access / Billing
        ↓
Backend Services

Workspace functionality must not depend on continuous synchronization of clients, appointments or payments with the backend.

Summary

The functional model of Aveli can be reduced to four main responsibilities:

Identity
   ↓
Access
   ↓
Local Workspace
   ↓
Daily Professional Workflow

The backend controls identity and entitlement, while the mobile client manages the user's operational workspace locally.