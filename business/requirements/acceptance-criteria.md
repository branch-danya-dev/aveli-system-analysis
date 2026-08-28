# Aveli — Acceptance Criteria

<p align="center"><a href="acceptance-criteria.md"><b>English</b></a> · <a href="acceptance-criteria.ru.md">Русский</a></p>

> Observable product conditions used to verify requirements and business rules.

## Status

**Baseline: Stable**

## Authentication

| ID | Acceptance Criterion |
|---|---|
| AC-001 | A new user can create an account and enter an authenticated state. |
| AC-002 | An existing user can sign in with valid credentials. |
| AC-003 | Invalid credentials do not create an authenticated session. |
| AC-004 | Logout ends the active authenticated state. |
| AC-005 | Logout does not delete the professional workspace. |

## Trial and Access

| ID | Acceptance Criterion |
|---|---|
| AC-006 | A newly created account receives one 30-day trial. |
| AC-007 | Logout/sign-in does not create a new trial. |
| AC-008 | Reinstall does not create a new trial for the same account. |
| AC-009 | At least one valid access source allows workspace entry. |
| AC-010 | No valid access source prevents workspace entry. |
| AC-011 | Valid access unlocks the workspace as a whole. |
| AC-012 | Access expiry preserves professional workspace information. |
| AC-013 | Restored valid access exposes the previously preserved workspace again. |

## Subscription

| ID | Acceptance Criterion |
|---|---|
| AC-014 | The user can start supported monthly/yearly purchase flows. |
| AC-015 | Monthly/yearly subscriptions provide the same logical access. |
| AC-016 | Subscription state can be reconciled with Aveli access. |
| AC-017 | A valid reconciled subscription grants workspace access. |
| AC-018 | Restore of a valid subscription restores access. |
| AC-019 | Displayed subscription price matches provider/platform pricing. |
| AC-020 | Recurring billing and management location are clearly presented. |

## Professional Workspace Data

| ID | Acceptance Criterion |
|---|---|
| AC-021 | Workspace entities remain usable without continuous backend sync. |
| AC-022 | Created workspace information remains associated with the active user's workspace. |
| AC-023 | One user's information is not shown in another user's active workspace. |
| AC-024 | Visit media is not exposed across users. |
| AC-025 | Logout closes active workspace context without deleting persistent data. |

## Clients

| ID | Acceptance Criterion |
|---|---|
| AC-026 | Create a client and find it in the directory. |
| AC-027 | Update a client and observe the updated values. |
| AC-028 | Archive and restore a client. |
| AC-029 | Open client profile and review professional history. |
| AC-030 | Import a permitted device contact without modifying the source contact. |

## Appointments and Visits

| ID | Acceptance Criterion |
|---|---|
| AC-031 | Create an appointment satisfying current scheduling rules. |
| AC-032 | Created appointment appears in relevant Today/Calendar views. |
| AC-033 | Appointment create/reschedule violating working-hours/conflict rules is rejected. |
| AC-034 | Reschedule appointment and observe updated time in relevant views. |
| AC-035 | Cancel appointment and it is no longer active scheduled work. |
| AC-036 | No-show remains distinguishable from cancelled/completed. |
| AC-037 | Complete a valid visit. |
| AC-038 | Supported notes/photos remain attached to completed visit context. |

## Payments

| ID | Acceptance Criterion |
|---|---|
| AC-039 | Record payment for a valid payable visit. |
| AC-040 | Completed visit can remain unpaid or partial. |
| AC-041 | Outstanding amount remains visible until resolved. |
| AC-042 | Period finance reflects workspace payment data. |

## Reminders

| ID | Acceptance Criterion |
|---|---|
| AC-043 | Create/use a supported appointment reminder. |
| AC-044 | Open valid reminder to existing related appointment. |
| AC-045 | Logout deactivates outgoing-user reminders. |
| AC-046 | Account switching does not expose previous-user reminders. |

## Offline Access

| ID | Acceptance Criterion |
|---|---|
| AC-047 | Previously verified access can permit offline workspace use while policy trusts it. |
| AC-048 | Offline access remains bounded by verification policy. |
| AC-049 | Expired verification requires renewed verification. |
| AC-050 | Access/backend verification failure does not delete/corrupt workspace data. |

## Final Coverage Additions

| ID | Acceptance Criterion |
|---|---|
| AC-056 | A valid stored refresh session restores authentication and opens the local workspace belonging to the restored user. |
| AC-057 | The user can create/update a service and use its current price/duration in appointment planning. |
| AC-058 | A service referenced by existing appointments cannot be removed in a way that invalidates historical appointment meaning. |
| AC-059 | Today shows the current daily appointment projection and reflects appointment lifecycle changes. |
| AC-060 | Calendar navigation changes the selected date and shows the corresponding appointment projection. |
| AC-061 | Supported local profile, schedule, appearance and currency settings can be changed and remain available according to their persistence model. |
| AC-062 | Russian and English localization can be selected without changing professional workspace ownership. |
| AC-063 | A supported workspace export can be produced and a supported import file can be selected/read; automatic conflict merge is not claimed. |
| AC-064 | One appointment cannot end with two independent payment records; partial/full progress remains within one payment aggregate. |

## Historical Identifier Note

`AC-051`–`AC-055` belonged to an earlier release-specific acceptance model. They remain intentionally unused.

Technical release verification is canonical in [`../../system/review/release-readiness.md`](../../system/review/release-readiness.md), [`../../frontend/testing/`](../../frontend/testing/), [`../../frontend/security/`](../../frontend/security/), and [`../../backend/security/`](../../backend/security/).

## Remaining Measurement Work

Numeric performance targets and per-release store/device evidence are classified in [`../../system/review/open-questions.md`](../../system/review/open-questions.md), not silently treated as product acceptance claims.

## Related Documentation

- [`business-rules.md`](business-rules.md)
- [`functional-requirements.md`](functional-requirements.md)
- [`non-functional-requirements.md`](non-functional-requirements.md)
- [`../traceability/`](../traceability/)
