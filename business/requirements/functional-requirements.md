# Aveli — Functional Requirements

<p align="center">
  <a href="functional-requirements.md"><b>English</b></a> ·
  <a href="functional-requirements.ru.md">Русский</a>
</p>

> Defines the observable capabilities and behavior Aveli must provide within the current product scope.

---

## Overview

Functional requirements describe **what the system must allow the user or product to do**.

They define expected behavior without prescribing technical implementation.

Example:

```text
The user must be able to restore an existing subscription.
```

This belongs in `business/requirements/`.

The implementation details belong in technical areas such as:

```text
backend/
frontend/
database/
integrations/
```

---

## Account and Authentication

| ID | Requirement |
|---|---|
| FR-001 | The system must allow a new user to create an account. |
| FR-002 | The system must allow an existing user to sign in. |
| FR-003 | The system must restore an authenticated user session when the current session can still be trusted. |
| FR-004 | The system must allow the user to log out. |
| FR-005 | The active authenticated user must determine which professional workspace is opened. |

Related business rules:

`BR-023`, `BR-043`–`BR-047`

---

## Trial and Access

| ID | Requirement |
|---|---|
| FR-006 | A newly created account must receive a 30-day trial. |
| FR-007 | The same account must keep its original trial state across logout, reinstall, and local workspace reset. |
| FR-008 | The system must determine whether the current authenticated user may open the workspace. |
| FR-009 | Workspace access may be granted by lifetime access, manual access, active subscription, or active trial. |
| FR-010 | The system must prevent entry to the workspace when no valid access source exists. |
| FR-011 | Access must be applied to the workspace as a whole rather than through independent feature-level paywalls. |
| FR-012 | Expiration of access must not remove existing professional workspace information. |
| FR-013 | Restoring valid access must make the existing workspace available again. |

Related business rules:

`BR-001`–`BR-013`, `BR-025`, `BR-026`

---

## Subscription

| ID | Requirement |
|---|---|
| FR-014 | The user must be able to start a supported monthly or yearly subscription purchase flow. |
| FR-015 | The user must be able to restore an existing purchase. |
| FR-016 | The system must reconcile subscription state with the current Aveli access state. |
| FR-017 | A valid subscription must grant subscription-based workspace access. |
| FR-018 | Monthly and yearly plans must provide the same logical workspace access level. |
| FR-019 | Subscription pricing shown to the user must reflect the current platform-provided price. |

Related business rules:

`BR-014`–`BR-019`

Provider-specific implementation belongs to:

`../../integrations/`

---

## Clients

| ID | Requirement |
|---|---|
| FR-020 | The user must be able to create a client. |
| FR-021 | The user must be able to update client information. |
| FR-022 | The user must be able to archive and restore a client. |
| FR-023 | The user must be able to delete a client when current client-lifecycle rules allow deletion. |
| FR-024 | The user must be able to browse and search the client directory. |
| FR-025 | The user must be able to open a client profile and review available professional history. |
| FR-026 | The user must be able to create an Aveli client from a device contact when permission is granted. |

Related business rules:

`BR-039`–`BR-042`

---

## Services

| ID | Requirement |
|---|---|
| FR-027 | The user must be able to create and update services. |
| FR-028 | A service must support the business information required for appointment planning, including price and expected duration. |
| FR-029 | The user must be able to deactivate or delete a service when current service-lifecycle rules allow it. |

---

## Appointments

| ID | Requirement |
|---|---|
| FR-030 | The user must be able to create an appointment. |
| FR-031 | An appointment must contain the client, date, time, and other required planning information. |
| FR-032 | The system must reject appointment states that violate the current scheduling rules. |
| FR-033 | The user must be able to reschedule an appointment. |
| FR-034 | The user must be able to cancel an appointment. |
| FR-035 | The user must be able to mark an appointment as no-show. |
| FR-036 | The user must be able to complete a visit. |
| FR-037 | A completed visit must allow supported professional context such as notes and photos to be preserved. |
| FR-038 | Appointment changes must be reflected in the relevant daily and calendar views. |

Related business rules:

`BR-027`–`BR-033`

Detailed scheduling rules should be documented separately once the exact slot and conflict model is finalized.

---

## Payments

| ID | Requirement |
|---|---|
| FR-039 | The user must be able to record payment for work that is valid for payment under the current visit lifecycle. |
| FR-040 | A completed visit must be allowed to remain unpaid. |
| FR-041 | The user must be able to review outstanding payments. |
| FR-042 | The system must provide basic period-based financial information based on recorded professional payments. |

Related business rules:

`BR-034`–`BR-038`

---

## Today and Calendar

| ID | Requirement |
|---|---|
| FR-043 | The application must provide a daily workspace view for the current day. |
| FR-044 | The application must provide calendar-based navigation through appointments. |
| FR-045 | The user must be able to move between supported calendar dates. |
| FR-046 | Today and Calendar must reflect the current appointment lifecycle after create, reschedule, cancel, no-show, and completion actions. |

---

## Reminders

| ID | Requirement |
|---|---|
| FR-047 | The user must be able to use reminders for supported appointments. |
| FR-048 | A reminder must remain associated with the relevant appointment context. |
| FR-049 | Reminders belonging to the outgoing user must no longer remain active after logout. |
| FR-050 | Opening a valid appointment reminder should navigate to the related appointment when the appointment still exists and is available. |

Related business rules:

`BR-053`–`BR-056`

Technical notification behavior belongs to:

`../../frontend/notifications/`

---

## Profile and Settings

| ID | Requirement |
|---|---|
| FR-051 | The user must be able to manage profile information. |
| FR-052 | The user must be able to configure their working schedule. |
| FR-053 | The user must be able to change the application language. |
| FR-054 | The application must support Russian and English localization. |
| FR-055 | The user must be able to configure supported appearance preferences. |
| FR-056 | The user must be able to configure the working currency used by supported product features. |
| FR-057 | The user must be able to export and import supported professional workspace data. |

---

## Professional Workspace Data

| ID | Requirement |
|---|---|
| FR-058 | Professional workspace information must remain available as part of the user's personal workspace without requiring continuous backend synchronization. |
| FR-059 | Professional workspace information must remain isolated between different authenticated users. |
| FR-060 | User-specific visit media must remain associated only with the corresponding user workspace. |
| FR-061 | Logout must not delete persistent professional workspace information. |
| FR-062 | Access expiration must not delete persistent professional workspace information. |

Related business rules:

`BR-020`–`BR-026`

Physical storage details belong to:

`../../database/`

---

## Offline Behavior

| ID | Requirement |
|---|---|
| FR-063 | The user must be able to continue normal professional workspace operations without permanent network connectivity. |
| FR-064 | Previously verified access may allow temporary workspace use while current verification is unavailable. |
| FR-065 | Offline workspace access must stop when the current access-verification policy no longer considers the previous verification sufficient. |
| FR-066 | Operations requiring current account, access, or subscription verification must require connectivity. |

Related business rules:

`BR-048`–`BR-052`

Technical implementation belongs to:

```text
../../frontend/offline/
../../backend/access/
```

---

## Requirement Boundaries

Functional requirements define expected behavior.

They must not contain implementation decisions such as:

```text
Use PostgreSQL
Use SQLite
Use JWT
Use RevenueCat SDK
Use REST
Use Docker
```

If such a decision is required, it belongs in the relevant technical documentation or architecture decision record.

A functional requirement may reference a technical area when the implementation needs further detail.

---

## Open Rule Areas

Several behaviors are intentionally kept at a high level until their detailed business rules are finalized.

These include:

- exact appointment conflict rules;
- exact client delete/archive rules;
- exact service delete/deactivate rules;
- exact payment lifecycle;
- exact export/import merge behavior;
- exact offline verification duration.

Once these rules are fixed, they should be documented in `business-rules` and reflected in the affected functional requirements without introducing implementation details.

---

## Related Documentation

- [`../scope/scope.md`](../scope/scope.md)
- [`business-rules.md`](business-rules.md)
- [`non-functional-requirements.md`](non-functional-requirements.md)
- [`acceptance-criteria.md`](acceptance-criteria.md)
- [`../traceability/`](../traceability/)
- [`../../database/`](../../database/)
- [`../../backend/`](../../backend/)
- [`../../frontend/`](../../frontend/)
- [`../../integrations/`](../../integrations/)
