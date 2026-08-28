# Aveli — Business Rules

<p align="center">
  <a href="business-rules.md"><b>English</b></a> ·
  <a href="business-rules.ru.md">Русский</a>
</p>

> Defines product-level rules and constraints that must remain true across Aveli workflows.

---

## Overview

Business rules describe **conditions the product must preserve regardless of how a specific screen, API, database, or service is implemented**.

They are different from functional requirements:

```text
Functional Requirement
    ↓
What capability the system must provide

Business Rule
    ↓
What condition or constraint must remain true
```

A business rule may affect several features at once.

For example:

```text
Loss of access must not delete workspace data.
```

This rule affects access behavior, local data handling, logout, subscription expiration, recovery, and acceptance testing.

Technical implementation is documented in the corresponding system areas.

---

## Access Rules

Aveli uses one workspace-level access model.

Different access sources may exist, but they all answer the same product question:

> **May the authenticated user open the workspace now?**

| ID | Rule |
|---|---|
| BR-001 | A user may open the workspace only when at least one valid access source exists. |
| BR-002 | Access sources are evaluated in the following priority: Lifetime → Manual Grant → Active Subscription → Active Trial → None. |
| BR-003 | Lifetime access has priority over all other access sources. |
| BR-004 | Manual access has priority over subscription and trial access. |
| BR-005 | Active subscription access has priority over active trial access. |
| BR-006 | If no valid access source exists, the workspace must remain unavailable. |
| BR-007 | Workspace access is granted or denied as a whole; individual workspace features are not independently paywalled in the current product model. |

The priority model exists to keep access behavior deterministic when several access sources are valid at the same time.

Detailed technical resolution belongs to:

`../../backend/access/`

---

## Trial Rules

The Aveli trial belongs to the user account rather than to a single installation of the application.

| ID | Rule |
|---|---|
| BR-008 | A new account receives one 30-day trial. |
| BR-009 | Trial state belongs to the user account and must not depend solely on local application state. |
| BR-010 | Logging out must not restart or extend the trial. |
| BR-011 | Reinstalling the application must not create a new trial for the same account. |
| BR-012 | Removing local workspace data must not create a new trial for the same account. |
| BR-013 | The current product model must not combine the Aveli trial with an additional store-managed free trial. |

The trial is therefore a product entitlement, not an installation-based counter.

---

## Subscription Rules

Aveli supports recurring subscription access through the mobile platform billing ecosystem.

| ID | Rule |
|---|---|
| BR-014 | Monthly and yearly subscription plans grant the same logical workspace access level. |
| BR-015 | Subscription prices shown to the user must reflect the current platform-provided price rather than an independently maintained static value. |
| BR-016 | A recurring subscription must be presented as recurring when the corresponding store product is configured as auto-renewing. |
| BR-017 | The user must be informed that subscription management and cancellation are handled through the corresponding mobile platform. |
| BR-018 | Completion of a purchase flow is not by itself sufficient to bypass the common Aveli access decision. |
| BR-019 | Restoring an existing valid subscription must restore the corresponding subscription-based workspace access. |

Provider-specific subscription behavior and integration details belong to:

`../../integrations/`

---

## Workspace Data Ownership Rules

Professional workspace information and commercial access are separate product concerns.

| ID | Rule |
|---|---|
| BR-020 | Clients, appointments, services, payments, visit notes, and visit photos belong to the professional workspace domain. |
| BR-021 | Professional workspace information is not synchronized between devices in the current product model. |
| BR-022 | Account and access responsibilities must remain separate from normal professional workspace ownership. |
| BR-023 | Each user must have an isolated professional workspace. |
| BR-024 | Visit photos and other user-specific workspace materials must not be exposed across different user workspaces. |
| BR-025 | Logging out must not delete the user's persistent professional workspace information. |
| BR-026 | Expiration of trial, subscription, or another access source must not delete or modify existing professional workspace information. |

These rules define the product ownership boundary.

The physical data model is documented in:

`../../database/`

---

## Appointment Rules

Appointments represent planned professional work and must remain consistent with the specialist's schedule and visit lifecycle.

| ID | Rule |
|---|---|
| BR-027 | An appointment must belong to a valid client. |
| BR-028 | An appointment must reference a valid service when service selection is required by the workflow. |
| BR-029 | Appointment date and time must satisfy the configured scheduling rules. |
| BR-030 | Conflicting appointments must be rejected according to the current slot-availability rules. |
| BR-031 | A cancelled appointment must not be treated as an active scheduled visit. |
| BR-032 | A no-show must remain distinguishable from a cancelled or completed visit. |
| BR-033 | A completed visit may contain notes, photos, and payment information. |

Detailed scheduling constraints are defined separately from these high-level rules so they can evolve without changing the business meaning of an appointment.

---

## Payment Rules

Aveli tracks payment state as part of completed professional work.

It is not intended to replace a full accounting system.

| ID | Rule |
|---|---|
| BR-034 | Payment may be recorded only for work that is valid for payment under the current visit lifecycle. |
| BR-035 | A completed visit may remain unpaid. |
| BR-036 | Outstanding payments must remain visible until they are resolved. |
| BR-037 | Payment state must remain consistent with the related visit state. |
| BR-038 | Financial summaries must be based on payment information recorded in the professional workspace. |

Detailed payment data structures belong to the database documentation.

---

## Client Rules

Client records belong to the active specialist workspace and must preserve professional history.

| ID | Rule |
|---|---|
| BR-039 | Client records belong only to the active user's professional workspace. |
| BR-040 | Archiving a client must preserve historical information unless the user explicitly performs an allowed deletion action. |
| BR-041 | Importing a device contact creates or enriches an Aveli client record and must not modify the original device contact as part of normal import behavior. |
| BR-042 | Client visit history must be derived from the professional activity associated with that client. |

Detailed delete/archive conditions should be maintained as explicit rules once the final client lifecycle is fixed.

---

## Session and Account Switching Rules

Authentication state and professional workspace ownership must remain separate.

| ID | Rule |
|---|---|
| BR-043 | Logging out must end the active authenticated session. |
| BR-044 | Logging out must leave the previously active workspace inactive until the same user authenticates again. |
| BR-045 | User-specific reminders associated with the outgoing account must no longer remain active after logout. |
| BR-046 | Logging out must not remove the outgoing user's persistent professional workspace information. |
| BR-047 | After another account becomes active, information belonging to the previous user must not be exposed in the new active workspace. |

Technical session lifecycle is documented in:

`../../backend/auth/`

---

## Offline Access Rules

Aveli is designed for offline-oriented daily work, but offline access must not become permanent unverified access.

| ID | Rule |
|---|---|
| BR-048 | A previously verified access state may temporarily allow the workspace to remain available without connectivity. |
| BR-049 | Offline access is valid only within the current offline verification policy. |
| BR-050 | When the offline verification period is exhausted, renewed access verification is required before continued workspace access. |
| BR-051 | Normal professional workspace operations must not require continuous network connectivity. |
| BR-052 | Operations that require current account, access, or subscription verification require connectivity. |

The exact offline verification mechanism and duration belong to the technical documentation:

`../../frontend/offline/`

`../../backend/access/`

---

## Reminder Rules

Reminders are part of the active user's workspace context.

| ID | Rule |
|---|---|
| BR-053 | Appointment reminders are created for appointments belonging to the active user workspace. |
| BR-054 | Reminders must not expose appointment information belonging to another user after account switching. |
| BR-055 | Logging out must deactivate reminders associated with the outgoing user. |
| BR-056 | Opening a valid appointment reminder should lead to the related appointment when that appointment still exists and can be opened. |

The notification implementation and platform behavior belong to:

`../../frontend/notifications/`

---

## Rule Relationships

Many rules intentionally affect more than one technical area.

Example:

```text
BR-026
Access expiration must not delete workspace information
        ↓
database/
frontend/
backend/access/
acceptance criteria
```

Another example:

```text
BR-047
Different users must not see each other's workspace
        ↓
database/
frontend/storage/
backend/auth/
reminders/
```

This is expected.

Business rules define the invariant.

Technical documents explain how each system component preserves it.

---

## Rules Moved Out of Business

Implementation-specific release and security constraints are not maintained as business rules.

Examples include:

```text
HTTPS enforcement
loopback-address restrictions
development-mode restrictions
secret placement
release configuration validation
```

These concerns are still important, but their canonical documentation belongs to:

```text
../../operations/
../../backend/security/
../../system/decisions/
```

If one of those constraints becomes contractually or product-significant, its business consequence may be referenced here without duplicating the implementation rule.

---

## Related Documentation

- [`../scope/scope.md`](../scope/scope.md)
- [`functional-requirements.md`](functional-requirements.md)
- [`non-functional-requirements.md`](non-functional-requirements.md)
- [`acceptance-criteria.md`](acceptance-criteria.md)
- [`../traceability/`](../traceability/)
- [`../../database/`](../../database/)
- [`../../backend/`](../../backend/)
- [`../../frontend/`](../../frontend/)
- [`../../integrations/`](../../integrations/)
