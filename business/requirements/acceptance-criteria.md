# Aveli — Acceptance Criteria

<p align="center">
  <a href="acceptance-criteria.md"><b>English</b></a> ·
  <a href="acceptance-criteria.ru.md">Русский</a>
</p>

> Defines observable conditions used to verify that Aveli behavior matches the agreed business rules, functional requirements, and product-level quality expectations.

---

## Overview

Acceptance criteria describe **what must be observable for a requirement or rule to be considered satisfied**.

They verify behavior without defining how that behavior is implemented.

Example:

```text
Requirement:
The user must be able to restore an existing purchase.

Acceptance:
A user with an existing valid subscription can restore it
and regain subscription-based workspace access.
```

Implementation-specific checks belong to the technical documentation and test suites.

---

## Authentication

| ID | Acceptance Criterion |
|---|---|
| AC-001 | A new user can create an account and continue into an authenticated product state. |
| AC-002 | An existing user can sign in with valid credentials. |
| AC-003 | Invalid credentials do not create an authenticated session. |
| AC-004 | Logout ends the active authenticated state. |
| AC-005 | Logout does not delete the user's existing professional workspace information. |

Related requirements:

`FR-001`–`FR-005`

Related business rules:

`BR-023`, `BR-043`–`BR-047`

---

## Trial and Access

| ID | Acceptance Criterion |
|---|---|
| AC-006 | A newly created account receives one 30-day trial. |
| AC-007 | Logging out and signing in again does not create a new trial for the same account. |
| AC-008 | Reinstalling the application does not create a new trial for the same account. |
| AC-009 | A user with at least one valid access source can open the workspace. |
| AC-010 | A user without any valid access source cannot open the workspace. |
| AC-011 | Valid access unlocks the workspace as a whole rather than independent paid feature groups. |
| AC-012 | Existing professional workspace information remains preserved after access expires. |
| AC-013 | Restoring valid access makes the previously preserved workspace available again. |

Related requirements:

`FR-006`–`FR-013`

Related business rules:

`BR-001`–`BR-013`, `BR-025`, `BR-026`

---

## Subscription

| ID | Acceptance Criterion |
|---|---|
| AC-014 | The user can start a supported monthly or yearly subscription purchase flow. |
| AC-015 | A valid monthly or yearly subscription results in the same logical workspace access level. |
| AC-016 | Subscription state can be reconciled with the current Aveli access state. |
| AC-017 | A valid reconciled subscription grants workspace access. |
| AC-018 | Restoring an existing valid subscription restores subscription-based workspace access. |
| AC-019 | Subscription pricing shown to the user matches the current platform-provided pricing information. |
| AC-020 | A recurring subscription is presented as recurring and the user is informed where subscription management is performed. |

Related requirements:

`FR-014`–`FR-019`

Related business rules:

`BR-014`–`BR-019`

---

## Professional Workspace Data

| ID | Acceptance Criterion |
|---|---|
| AC-021 | Clients, appointments, services, payments, notes, and supported visit media remain usable without continuous backend synchronization. |
| AC-022 | Workspace information created by the active user remains associated with that user's workspace. |
| AC-023 | Information from one user is not shown when another user's workspace is active. |
| AC-024 | User-specific visit media is not exposed across different user workspaces. |
| AC-025 | Logout closes the current active workspace context without deleting persistent professional information. |

Related requirements:

`FR-058`–`FR-062`

Related business rules:

`BR-020`–`BR-026`

---

## Clients

| ID | Acceptance Criterion |
|---|---|
| AC-026 | The user can create a client and then find that client in the directory. |
| AC-027 | The user can update an existing client and see the updated information afterwards. |
| AC-028 | The user can archive and later restore a client according to the current client lifecycle. |
| AC-029 | The user can open a client profile and review the available professional history associated with that client. |
| AC-030 | When device-contact access is permitted, the user can create an Aveli client from a selected device contact without modifying the original contact as part of normal import behavior. |

Related requirements:

`FR-020`–`FR-026`

Related business rules:

`BR-039`–`BR-042`

---

## Appointments and Visits

| ID | Acceptance Criterion |
|---|---|
| AC-031 | The user can create an appointment with all information required by the current scheduling rules. |
| AC-032 | A created appointment appears in the relevant Today and Calendar views. |
| AC-033 | An appointment state that violates the current scheduling rules is rejected. |
| AC-034 | The user can reschedule an existing appointment and the updated time is reflected in relevant views. |
| AC-035 | The user can cancel an appointment and it is no longer treated as an active scheduled visit. |
| AC-036 | The user can mark an appointment as no-show and that state remains distinguishable from cancelled and completed states. |
| AC-037 | The user can complete a valid visit. |
| AC-038 | Supported notes and visit photos can be preserved as part of the completed visit context. |

Related requirements:

`FR-030`–`FR-038`

Related business rules:

`BR-027`–`BR-033`

---

## Payments

| ID | Acceptance Criterion |
|---|---|
| AC-039 | The user can record payment for work that is valid for payment under the current visit lifecycle. |
| AC-040 | A completed visit can remain unpaid without becoming an invalid visit state. |
| AC-041 | Outstanding payments remain visible until they are resolved. |
| AC-042 | Period-based financial information reflects payment information recorded in the professional workspace. |

Related requirements:

`FR-039`–`FR-042`

Related business rules:

`BR-034`–`BR-038`

---

## Reminders

| ID | Acceptance Criterion |
|---|---|
| AC-043 | The user can create a supported reminder for an appointment. |
| AC-044 | Opening a valid appointment reminder leads to the related appointment when that appointment still exists and is available. |
| AC-045 | Logging out deactivates reminders associated with the outgoing user. |
| AC-046 | After account switching, reminders do not expose appointment information belonging to another user. |

Related requirements:

`FR-047`–`FR-050`

Related business rules:

`BR-053`–`BR-056`

---

## Offline Access

| ID | Acceptance Criterion |
|---|---|
| AC-047 | A user with previously verified access can continue using the workspace offline while that verification is still considered sufficient. |
| AC-048 | Offline workspace access remains available only within the current verification policy. |
| AC-049 | When previous verification is no longer sufficient, continued workspace access requires renewed verification. |
| AC-050 | Failure to verify access or temporary unavailability of account services does not delete or corrupt existing professional workspace information. |

Related requirements:

`FR-063`–`FR-066`

Related business rules:

`BR-048`–`BR-052`

---

## Product-Level Quality Verification

The following quality characteristics must also be verified through repeatable product-level scenarios:

### Access Consistency

The same account and access state must not produce contradictory workspace availability across startup, resume, purchase, restore, and normal navigation.

Related:

`NFR-019`–`NFR-022`

### User Isolation

Changing the active account must not expose the previous user's workspace information, visit media, or reminders.

Related:

`NFR-008`, `NFR-009`, `NFR-027`, `NFR-032`

### Data Continuity

Logout, expired access, temporary backend failure, or failed subscription reconciliation must not remove existing professional workspace information.

Related:

`NFR-010`, `NFR-028`–`NFR-031`

### Usability

Trial and subscription state must be understandable to the user and must not present misleading access or billing information.

Related:

`NFR-037`–`NFR-042`

Exact performance thresholds and low-level technical verification are maintained in the corresponding technical areas.

---

## Technical Verification Moved Out of Business

The previous version of this document used `AC-051`–`AC-055` for implementation-specific release checks.

Those checks are no longer canonical business acceptance criteria.

They covered concerns such as:

```text
loopback / emulator endpoint rejection
development-only mode rejection
production transport configuration
secret placement
release configuration validation
```

These checks will be maintained as technical verification in:

```text
../../operations/release/
../../operations/testing/
../../operations/configuration/
../../backend/security/
```

The identifiers `AC-051`–`AC-055` are intentionally not reassigned in this document to avoid confusing historical traceability.

---

## Acceptance Boundary

Acceptance criteria in `business/` should verify **observable product behavior**.

They should not depend on implementation details unless the implementation itself is part of an agreed external contract.

Avoid criteria such as:

```text
A PostgreSQL row exists.
A JWT contains field X.
A Flutter provider changes state.
A Docker container starts.
```

Prefer:

```text
The user remains signed in after a valid session restoration.

The workspace is unavailable when no valid access source exists.

Existing user information remains available after access is restored.
```

Technical tests may verify deeper implementation details separately.

---

## Open Acceptance Areas

Some criteria cannot be made fully precise until the corresponding business rules are finalized.

These include:

- exact appointment-conflict scenarios;
- exact client delete/archive behavior;
- exact service delete/deactivate behavior;
- exact payment lifecycle and duplicate-payment handling;
- export/import conflict behavior;
- measurable performance thresholds;
- exact offline verification duration.

When those rules are finalized, the related acceptance criteria should be extended without changing unrelated criteria.

---

## Related Documentation

- [`../scope/scope.md`](../scope/scope.md)
- [`functional-requirements.md`](functional-requirements.md)
- [`non-functional-requirements.md`](non-functional-requirements.md)
- [`business-rules.md`](business-rules.md)
- [`../traceability/`](../traceability/)
- [`../../operations/testing/`](../../operations/testing/)
