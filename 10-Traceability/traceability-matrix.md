# Aveli — Traceability Matrix

## Purpose

This document connects business rules, functional requirements and acceptance criteria.

The goal is to show that key system behavior can be traced from rule → requirement → verification.

---

## Access and Trial

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-001, BR-002 | FR-008, FR-009 | AC-009 |
| BR-006, BR-007 | FR-010, FR-011 | AC-010, AC-011 |
| BR-008, BR-009 | FR-006, FR-007 | AC-006 |
| BR-010, BR-011, BR-012 | FR-007 | AC-007, AC-008 |
| BR-025, BR-026 | FR-012, FR-013 | AC-012, AC-013 |

---

## Subscription

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-014 | FR-018 | AC-015 |
| BR-015 | FR-019 | AC-019 |
| BR-016, BR-017 | FR-014 | AC-020 |
| BR-018 | FR-016, FR-017 | AC-016, AC-017 |
| BR-019 | FR-015 | AC-018 |

---

## Authentication and Session

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-043 | FR-004 | AC-004 |
| BR-044 | FR-004, FR-005 | AC-005 |
| BR-046 | FR-004 | AC-005 |
| BR-047 | FR-005, FR-059 | AC-023 |

---

## Clients

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-039 | FR-020, FR-021 | AC-026, AC-027 |
| BR-040 | FR-022 | AC-028 |
| BR-041 | FR-026 | AC-030 |
| BR-042 | FR-025 | AC-029 |

---

## Appointments

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-027, BR-028 | FR-030, FR-031 | AC-031 |
| BR-029, BR-030 | FR-032 | AC-033 |
| BR-031 | FR-034 | AC-035 |
| BR-032 | FR-035 | AC-036 |
| BR-033 | FR-036, FR-037 | AC-037, AC-038 |

---

## Payments

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-034 | FR-039 | AC-039 |
| BR-035, BR-036 | FR-040, FR-041 | AC-040, AC-041 |
| BR-037 | FR-039, FR-040 | AC-039, AC-040 |
| BR-038 | FR-042 | AC-042 |

---

## Local Data Ownership

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-020 | FR-058 | AC-021, AC-022 |
| BR-021, BR-022 | FR-058 | AC-021 |
| BR-023 | FR-059 | AC-022, AC-023 |
| BR-024 | FR-060 | AC-024 |
| BR-025, BR-026 | FR-061, FR-062 | AC-025, AC-012 |

---

## Offline Behavior

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-048 | FR-064 | AC-047 |
| BR-049 | FR-065 | AC-048 |
| BR-050 | FR-065 | AC-049 |
| BR-051 | FR-063 | AC-021 |
| BR-052 | FR-066 | AC-050 |

---

## Reminders

| Business Rule | Functional Requirement | Acceptance Criteria |
|---|---|---|
| BR-053 | FR-047, FR-048 | AC-043 |
| BR-054, BR-055 | FR-049 | AC-045, AC-046 |
| BR-056 | FR-050 | AC-044 |

---

## Release Safety

| Business Rule | Functional Requirement / Constraint | Acceptance Criteria |
|---|---|---|
| BR-057 | NFR-053, NFR-055 | AC-053, AC-055 |
| BR-058 | NFR-053 | AC-051 |
| BR-059 | NFR-054 | AC-052 |
| BR-060 | NFR-016, NFR-017 | AC-054 |
| BR-061 | NFR-052, NFR-055 | AC-055 |

---

## Example Trace

A complete trace can be read as:

```text
BR-008
30-day trial is created once on server registration
        ↓
FR-006
Backend must create a 30-day trial for a new account
        ↓
AC-006
Newly registered account receives a 30-day trial

Another example:

BR-023
Each account uses a separate local workspace database
        ↓
FR-059
Local data must be isolated by authenticated user
        ↓
AC-023
Data from one account is not shown to another account
Coverage View

The matrix covers the main areas of the current system:

Authentication
Access
Trial
Subscription
Clients
Appointments
Payments
Local Data
Offline Mode
Reminders
Release Safety
Summary

The traceability chain used in Aveli is:

Business Rule
     ↓
Functional Requirement
     ↓
Acceptance Criterion

This keeps system behavior connected from design decision to verifiable outcome.