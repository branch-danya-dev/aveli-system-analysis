# Aveli — Functional Requirements

<p align="center"><a href="functional-requirements.md"><b>English</b></a> · <a href="functional-requirements.ru.md">Русский</a></p>

> Observable product capabilities inside the current Aveli scope.

## Status

**Baseline: Stable**

## Account and Authentication

| ID | Requirement |
|---|---|
| FR-001 | Allow a new user to create an account. |
| FR-002 | Allow an existing user to sign in. |
| FR-003 | Restore an authenticated session when the current refresh session can still be trusted. |
| FR-004 | Allow the user to log out. |
| FR-005 | Use the active authenticated user to select the professional workspace. |

## Trial and Access

| ID | Requirement |
|---|---|
| FR-006 | Give a newly created account one 30-day trial. |
| FR-007 | Preserve the original account trial across logout, reinstall, and local workspace reset. |
| FR-008 | Determine whether the current authenticated user may open the workspace. |
| FR-009 | Support lifetime, manual, subscription, and trial access sources. |
| FR-010 | Prevent workspace entry when no valid access source exists. |
| FR-011 | Apply access to the workspace as a whole rather than feature-level paywalls. |
| FR-012 | Preserve professional workspace data when access expires. |
| FR-013 | Make the existing workspace available again after valid access is restored. |

## Subscription

| ID | Requirement |
|---|---|
| FR-014 | Start supported monthly/yearly subscription purchase flows. |
| FR-015 | Restore an existing purchase. |
| FR-016 | Reconcile provider subscription state with Aveli access state. |
| FR-017 | Grant subscription-based workspace access after a valid reconciled subscription. |
| FR-018 | Treat monthly/yearly plans as the same logical access level. |
| FR-019 | Show current platform/provider subscription pricing. |

## Clients

| ID | Requirement |
|---|---|
| FR-020 | Create a client. |
| FR-021 | Update client information. |
| FR-022 | Archive and restore a client. |
| FR-023 | Delete a client only when current lifecycle rules allow deletion and no appointment history must be preserved through that client. |
| FR-024 | Browse and search the client directory. |
| FR-025 | Open a client profile and review available professional history. |
| FR-026 | Create or enrich an Aveli client from a device contact when permission is granted. |

## Services

| ID | Requirement |
|---|---|
| FR-027 | Create and update services. |
| FR-028 | Store service information required for planning, including price and expected duration. |
| FR-029 | Delete a service only when its removal does not invalidate existing appointment history; the current product has no separate service-deactivation state. |

## Appointments

| ID | Requirement |
|---|---|
| FR-030 | Create an appointment. |
| FR-031 | Require the client, date/time, service and other planning information needed by the current workflow. |
| FR-032 | Reject appointment creation/rescheduling outside the configured working schedule or in conflict with another active scheduled appointment under the current slot model. |
| FR-033 | Reschedule an appointment. |
| FR-034 | Cancel an appointment. |
| FR-035 | Mark an appointment as no-show. |
| FR-036 | Complete a visit. |
| FR-037 | Preserve supported visit context such as notes and photos. |
| FR-038 | Reflect appointment lifecycle changes in Today and Calendar. |

## Payments

| ID | Requirement |
|---|---|
| FR-039 | Record payment for work valid for payment under the visit lifecycle. |
| FR-040 | Allow a completed visit to remain unpaid or partially paid. |
| FR-041 | Review outstanding payments. |
| FR-042 | Provide basic period financial information from recorded workspace payments. |

## Today and Calendar

| ID | Requirement |
|---|---|
| FR-043 | Provide a daily workspace view for the current day. |
| FR-044 | Provide calendar-based appointment navigation. |
| FR-045 | Move between supported calendar dates. |
| FR-046 | Keep Today/Calendar projections consistent with appointment lifecycle changes. |

## Reminders

| ID | Requirement |
|---|---|
| FR-047 | Use reminders for supported appointments. |
| FR-048 | Associate each reminder with the relevant appointment context. |
| FR-049 | Deactivate outgoing-user reminders on logout. |
| FR-050 | Navigate from a valid reminder to the related existing appointment. |

## Profile and Settings

| ID | Requirement |
|---|---|
| FR-051 | Manage supported local profile information. |
| FR-052 | Configure the working schedule. |
| FR-053 | Change application language. |
| FR-054 | Support Russian and English localization. |
| FR-055 | Configure supported appearance preferences. |
| FR-056 | Configure the working currency used by supported product features. |
| FR-057 | Export and import supported workspace data as a user-mediated transfer; automatic merge of divergent workspace copies is outside the current stable contract. |

## Professional Workspace Data

| ID | Requirement |
|---|---|
| FR-058 | Keep professional workspace information usable without continuous backend synchronization. |
| FR-059 | Isolate professional workspace information between authenticated users. |
| FR-060 | Associate user-specific visit media only with the corresponding workspace. |
| FR-061 | Preserve persistent professional workspace information on logout. |
| FR-062 | Preserve persistent professional workspace information on access expiry. |

## Offline Behavior

| ID | Requirement |
|---|---|
| FR-063 | Continue normal professional workspace operations without permanent connectivity. |
| FR-064 | Allow temporary offline workspace access from previously verified access when policy permits. |
| FR-065 | Require renewed verification when the current offline policy no longer trusts the previous verification. |
| FR-066 | Require connectivity for operations needing current account/access/subscription verification. |

## Requirement Boundary

Functional requirements define product behavior, not framework/schema choices.

Current lifecycle clarifications are canonical in [`business-rules.md`](business-rules.md). Low-level interval comparison semantics, provider/store configuration, numeric performance targets, and future automatic workspace merge are not silently invented here.

## Related Documentation

- [`business-rules.md`](business-rules.md)
- [`non-functional-requirements.md`](non-functional-requirements.md)
- [`acceptance-criteria.md`](acceptance-criteria.md)
- [`../traceability/`](../traceability/)
- [`../../system/review/open-questions.md`](../../system/review/open-questions.md)
