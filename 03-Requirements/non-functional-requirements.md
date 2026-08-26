# Aveli — Non-Functional Requirements

## Purpose

This document defines quality attributes and technical constraints that shape Aveli behavior.

The requirements focus on reliability, security, privacy, usability, offline operation and release safety.

---

## Availability and Offline Operation

| ID | Requirement |
|---|---|
| NFR-001 | Core workspace operations must remain usable without continuous network connectivity. |
| NFR-002 | Local clients, appointments, services, payments, notes and photos must remain available while offline. |
| NFR-003 | Temporary backend unavailability must not immediately block a user with a valid cached access state. |
| NFR-004 | Offline access must be limited by the configured access verification policy. |
| NFR-005 | Operations that require backend state must clearly fail or wait when connectivity is unavailable. |

---

## Data Privacy

| ID | Requirement |
|---|---|
| NFR-006 | Operational client data must remain stored locally on the user's device. |
| NFR-007 | Work data must not be uploaded to the backend as part of normal application operation. |
| NFR-008 | Local databases must be isolated by authenticated user. |
| NFR-009 | Visit photos must be stored in user-specific local storage. |
| NFR-010 | Logout or access expiration must not delete operational work data. |

---

## Security

| ID | Requirement |
|---|---|
| NFR-011 | Authentication tokens must be stored using secure device storage. |
| NFR-012 | Cached access state must be stored in secure device storage. |
| NFR-013 | Backend authentication must use access and refresh token mechanisms. |
| NFR-014 | Refresh tokens must support rotation. |
| NFR-015 | Password storage must use a strong password hashing algorithm. |
| NFR-016 | Backend secrets must not be embedded in the mobile client. |
| NFR-017 | RevenueCat secret credentials and webhook authentication data must remain on the backend. |
| NFR-018 | Production API communication must use HTTPS. |

---

## Access Consistency

| ID | Requirement |
|---|---|
| NFR-019 | Workspace access must be determined through one consistent access decision mechanism. |
| NFR-020 | Individual screens must not independently implement conflicting entitlement checks. |
| NFR-021 | Access state must remain consistent across application startup, resume, purchase and restore flows. |
| NFR-022 | Reinstall or local data reset must not incorrectly reset the server-controlled trial. |

---

## Data Integrity

| ID | Requirement |
|---|---|
| NFR-023 | Local database operations must preserve consistency between appointments, clients, services and payments. |
| NFR-024 | Database migrations must preserve existing user data. |
| NFR-025 | Invalid scheduling states must be rejected before persistence. |
| NFR-026 | Payment operations must not create contradictory payment states. |
| NFR-027 | User data belonging to one account must not be exposed when another account is active. |

---

## Reliability

| ID | Requirement |
|---|---|
| NFR-028 | Application startup must handle missing or expired sessions safely. |
| NFR-029 | Failed backend verification must not corrupt local workspace data. |
| NFR-030 | Failed billing synchronization must leave the access state recoverable through a later refresh. |
| NFR-031 | Application termination during normal local operations must not invalidate the local database. |
| NFR-032 | Reminder state must be cleaned up when the active account changes. |

---

## Performance

| ID | Requirement |
|---|---|
| NFR-033 | Main workspace screens should load from local storage without depending on network latency. |
| NFR-034 | Calendar and daily schedule interactions should provide immediate local feedback. |
| NFR-035 | Local search and client navigation should remain responsive for the expected personal-workspace dataset size. |
| NFR-036 | Access verification must not unnecessarily block application startup when a valid cached state can be used. |

---

## Usability

| ID | Requirement |
|---|---|
| NFR-037 | Primary workflows must be accessible without exposing technical account or billing complexity to the user. |
| NFR-038 | Subscription messaging must clearly describe the recurring nature of the purchase. |
| NFR-039 | The interface must avoid misleading premium or trial language. |
| NFR-040 | The UI must support Russian and English localization. |
| NFR-041 | Motion and ambient effects must not prevent normal application interaction. |
| NFR-042 | Reduced-motion preferences should be respected where supported. |

---

## Maintainability

| ID | Requirement |
|---|---|
| NFR-043 | Client functionality should remain separated into presentation, domain and data responsibilities. |
| NFR-044 | Feature modules should remain independently maintainable where practical. |
| NFR-045 | Account/access logic must remain separated from operational workspace data logic. |
| NFR-046 | External integration logic must be isolated behind dedicated services or repositories. |
| NFR-047 | Business rules should be testable independently from UI behavior. |

---

## Testability

| ID | Requirement |
|---|---|
| NFR-048 | Access decisions must be covered by automated tests. |
| NFR-049 | Authentication and session lifecycle behavior must be testable. |
| NFR-050 | Database migrations must be covered by automated tests. |
| NFR-051 | Core appointment and payment business rules must be covered by automated tests. |
| NFR-052 | Release configuration constraints must be automatically verifiable. |

---

## Release Safety

| ID | Requirement |
|---|---|
| NFR-053 | Production builds must not use emulator or loopback API addresses. |
| NFR-054 | Production builds must not enable standalone development mode. |
| NFR-055 | Required production configuration must be validated before release. |
| NFR-056 | Public mobile SDK keys may be included in the client only where explicitly supported by the provider. |
| NFR-057 | Secret backend credentials must be provided through server-side configuration. |

---

## Architectural Constraint

Aveli must preserve the separation:

```text
Local Work Data
      ≠
Server Account Data

This is both a privacy requirement and an architectural constraint.

The backend may determine whether the user can access the workspace, but it must not become the operational source of truth for client, appointment or payment data in the current architecture.

Summary

The key quality goals of Aveli are:

Offline-first
    +
Privacy
    +
Access consistency
    +
Local data integrity
    +
Release safety

These constraints define how the system should behave even when connectivity, billing state or user access changes.