# Aveli — Traceability Matrix

<p align="center">
  <a href="traceability-matrix.md"><b>English</b></a> ·
  <a href="traceability-matrix.ru.md">Русский</a>
</p>

> Connects Aveli business rules, requirements, acceptance criteria, quality expectations, and technical ownership.

---

## Purpose

Traceability in Aveli is broader than a traditional:

```text
Requirement
    ↓
Test
```

The target model is:

```text
Business Intent
    ↓
Business Rule
    ↓
Functional Requirement
    ↓
Quality Constraint
    ↓
Acceptance Criterion
    ↓
Technical Ownership
```

The matrix does not redefine any of these artifacts.

It only records their relationships.

---

## Coverage Status

The matrix uses three states:

| Status | Meaning |
|---|---|
| **Covered** | The important business behavior has a meaningful path from requirement or rule to verification and technical ownership. |
| **Partial** | The behavior exists, but at least one important trace link, measurable criterion, or final rule is still missing. |
| **Open** | The product rule itself is not finalized enough for a stable trace. |

A missing mapping is intentionally visible.

Traceability should reveal uncertainty, not hide it.

---

## Core Product Traceability

| Area | Business Rules | Functional Requirements | Relevant NFR | Acceptance Criteria | Technical Ownership | Status |
|---|---|---|---|---|---|---|
| Authentication | BR-043–BR-047 | FR-001–FR-005 | NFR-011, NFR-013–NFR-015, NFR-049 | AC-001–AC-005 | `backend/auth/`, `frontend/bootstrap/` | **Partial** |
| Access decision | BR-001–BR-007 | FR-008–FR-011 | NFR-019–NFR-021 | AC-009–AC-011 | `backend/access/`, `frontend/bootstrap/` | **Covered** |
| Trial lifecycle | BR-008–BR-013 | FR-006–FR-007 | NFR-022 | AC-006–AC-008 | `backend/access/`, `database/server/` | **Covered** |
| Access expiration & restoration | BR-025–BR-026 | FR-012–FR-013, FR-061–FR-062 | NFR-010, NFR-028–NFR-029 | AC-012–AC-013, AC-025, AC-050 | `backend/access/`, `frontend/storage/`, `database/` | **Covered** |
| Subscription | BR-014–BR-019 | FR-014–FR-019 | NFR-021, NFR-030, NFR-038–NFR-039 | AC-014–AC-020 | `integrations/`, `backend/billing/`, `backend/access/`, `frontend/` | **Covered** |
| Client management | BR-039–BR-040, BR-042 | FR-020–FR-025 | NFR-023, NFR-027, NFR-035 | AC-026–AC-029 | `frontend/workspace/clients/`, `database/local/` | **Partial** |
| Device contact import | BR-041 | FR-026 | NFR-006–NFR-009 | AC-030 | `integrations/device-contacts/`, `frontend/workspace/clients/` | **Covered** |
| Services | — | FR-027–FR-029 | NFR-023 | — | `frontend/workspace/services/`, `database/local/` | **Open** |
| Appointments & visits | BR-027–BR-033 | FR-030–FR-038 | NFR-023, NFR-025, NFR-034 | AC-031–AC-038 | `frontend/workspace/appointments/`, `database/local/` | **Partial** |
| Payments | BR-034–BR-038 | FR-039–FR-042 | NFR-023, NFR-026 | AC-039–AC-042 | `frontend/workspace/payments/`, `database/local/` | **Partial** |
| Today & Calendar | — | FR-043–FR-046 | NFR-033–NFR-034 | AC-032 indirectly | `frontend/workspace/today/`, `frontend/workspace/calendar/` | **Partial** |
| Reminders | BR-053–BR-056 | FR-047–FR-050 | NFR-032 | AC-043–AC-046 | `frontend/notifications/` | **Covered** |
| Profile & settings | — | FR-051–FR-057 | NFR-040–NFR-042 | — | `frontend/workspace/settings/`, `frontend/localization/`, `database/local/` | **Partial** |
| Workspace ownership & isolation | BR-020–BR-026 | FR-058–FR-062 | NFR-006–NFR-010, NFR-023–NFR-024, NFR-027 | AC-021–AC-025 | `database/`, `frontend/storage/`, `backend/auth/` | **Covered** |
| Offline workspace | BR-048–BR-052 | FR-063–FR-066 | NFR-001–NFR-005, NFR-028–NFR-029, NFR-036 | AC-047–AC-050 | `frontend/offline/`, `backend/access/` | **Partial** |

---

## Detailed Access Trace

Access is one of the most cross-cutting parts of the product and demonstrates the intended traceability model.

```text
BR-001..BR-007
Workspace is available only when a valid access source exists
        ↓
FR-008..FR-011
The product must evaluate and enforce workspace-level access
        ↓
NFR-019..NFR-021
The decision must remain consistent across product states
        ↓
AC-009..AC-011
Allowed users enter the workspace; denied users reach Access Gate
        ↓
backend/access/
frontend/bootstrap/
```

Access expiration extends the trace:

```text
BR-026
Expiration must not delete professional workspace information
        ↓
FR-012 / FR-062
Access expiration preserves existing professional data
        ↓
NFR-010 / NFR-029
Loss or failure of access verification must not destroy workspace information
        ↓
AC-012 / AC-050
Existing information remains preserved
        ↓
backend/access/
frontend/storage/
database/
```

---

## Detailed User Isolation Trace

User isolation crosses authentication, data ownership, local storage, and reminders.

```text
BR-023 / BR-024 / BR-047 / BR-054
User workspaces and user-specific context must remain isolated
        ↓
FR-005 / FR-059 / FR-060 / FR-049
The active account determines workspace ownership and user-specific context
        ↓
NFR-008 / NFR-009 / NFR-027 / NFR-032
Information must never leak between active user contexts
        ↓
AC-023 / AC-024 / AC-045 / AC-046
Account switching and logout do not expose another user's data or reminders
        ↓
database/
frontend/storage/
frontend/notifications/
backend/auth/
```

This is an example of one business invariant affecting several technical components.

---

## Non-Functional Traceability

Not every NFR maps naturally to one business rule or one acceptance criterion.

Quality requirements are also traced to the technical area responsible for making them measurable.

| Quality Area | NFR | Current Verification | Technical Ownership | Status |
|---|---|---|---|---|
| Offline availability | NFR-001–NFR-005 | AC-047–AC-050 plus workspace scenarios | `frontend/offline/`, `backend/access/` | **Partial** |
| Privacy & workspace ownership | NFR-006–NFR-010 | AC-021–AC-025 | `database/`, `frontend/storage/` | **Covered** |
| Security | NFR-011–NFR-018 | Authentication/product scenarios + future technical security checks | `backend/security/`, `operations/security/` | **Partial** |
| Access consistency | NFR-019–NFR-022 | AC-006–AC-013, AC-016–AC-018 | `backend/access/`, `frontend/bootstrap/` | **Covered** |
| Data integrity | NFR-023–NFR-027 | Client, appointment, payment, and isolation acceptance scenarios | `database/`, relevant workspace modules | **Partial** |
| Reliability & recovery | NFR-028–NFR-032 | AC-023–AC-025, AC-045–AC-050 | relevant technical components | **Partial** |
| Performance | NFR-033–NFR-036 | Measurable thresholds not yet fixed | `frontend/`, `operations/testing/` | **Open** |
| Usability | NFR-037–NFR-042 | Subscription/access scenarios; localization and motion verification pending | `frontend/` | **Partial** |
| Maintainability | NFR-043–NFR-047 | Architecture review and automated checks to be defined | `system/`, component architecture | **Open** |
| Testability | NFR-048–NFR-052 | Technical test strategy not yet documented | `operations/testing/` | **Open** |

---

## Known Traceability Gaps

The current matrix intentionally exposes several incomplete areas.

### Authentication Session Restoration

`FR-003` defines session restoration, but the current acceptance document does not contain a dedicated acceptance criterion for successful session restoration.

A dedicated criterion should be added when the session lifecycle is finalized.

### Services

`FR-027`–`FR-029` describe service management.

Dedicated business rules and acceptance criteria for service lifecycle behavior are not yet defined.

### Appointment Conflict Model

The appointment flow is covered, but the exact slot-conflict model remains open.

`BR-029`, `BR-030`, `FR-032`, and `AC-033` cannot become fully precise until scheduling rules are finalized.

### Client Lifecycle

Archive/restore behavior exists, but final delete conditions remain open.

### Payment Lifecycle

Basic payment behavior is covered, but duplicate payment handling and the complete payment-state lifecycle are not yet finalized.

### Today and Calendar

`FR-043`–`FR-046` define the primary views, but dedicated acceptance criteria for their behavior are incomplete.

### Profile, Settings, Export and Import

`FR-051`–`FR-057` currently have no dedicated acceptance criteria.

Export/import behavior also requires explicit conflict and compatibility rules.

### Performance

`NFR-033`–`NFR-036` intentionally remain non-measurable until supported device classes, expected data volumes, and target thresholds are defined.

### Offline Verification Duration

The product behavior is defined, but the exact duration and verification policy remain technical/open decisions.

---

## Historical Migration

The previous traceability model contained a `Release Safety` section that linked:

```text
BR-057..BR-061
NFR-053..NFR-055
AC-051..AC-055
```

Those items described implementation-specific release and security constraints.

Under the current repository rules, they are no longer maintained as canonical business artifacts.

Their responsibilities move to:

```text
operations/release/
operations/testing/
operations/configuration/
backend/security/
system/decisions/
```

`AC-051`–`AC-055` remain intentionally unused in the business acceptance document so historical references are not silently reassigned.

---

## Traceability Direction

A trace can be followed in either direction.

### From Business to Implementation

```text
Why must this behavior exist?
    ↓
Which rule defines it?
    ↓
Which requirement expresses it?
    ↓
How is it verified?
    ↓
Which component owns the implementation?
```

### From Technology to Business

```text
Why does this component exist?
    ↓
Which requirement does it support?
    ↓
Which business rule or product behavior depends on it?
```

This second direction becomes increasingly important as technical documentation is added.

It allows architecture and technology choices to be justified by product needs rather than existing as isolated implementation facts.

---

## Future Machine-Readable Traceability

The human-readable matrix is the current canonical traceability view.

The methodology is designed to later support machine-readable relationships such as:

```yaml
id: TRACE-ACCESS-001

business_rules:
  - BR-001
  - BR-002

functional_requirements:
  - FR-008
  - FR-009

non_functional_requirements:
  - NFR-019

acceptance_criteria:
  - AC-009

technical_owners:
  - backend/access
  - frontend/bootstrap

status: covered
```

This can later support automated repository checks, dependency graphs, coverage reports, and AI-assisted analysis.

---

## Related Documentation

- [`../requirements/business-rules.md`](../requirements/business-rules.md)
- [`../requirements/functional-requirements.md`](../requirements/functional-requirements.md)
- [`../requirements/non-functional-requirements.md`](../requirements/non-functional-requirements.md)
- [`../requirements/acceptance-criteria.md`](../requirements/acceptance-criteria.md)
- [`../processes/`](../processes/)
- [`../../rules.md`](../../rules.md)
