# Traceability

> Navigation and ownership rules for Aveli requirement traceability.

---

## Purpose

The `traceability/` directory connects **business intent, requirements, verification, and technical ownership**.

Its purpose is not to duplicate requirements. It provides links between canonical documents so that an important product decision can be followed across abstraction levels.

---

## Responsibility

This area is responsible for showing relationships such as:

```text
Business Rule
    ↓
Functional Requirement
    ↓
Non-Functional Requirement
    ↓
Acceptance Criterion
    ↓
Technical Area
```

Not every relationship requires every level. A trace should contain only meaningful links.

---

## Boundaries

Canonical definitions remain in their owning documents:

```text
requirements/
backend/
frontend/
database/
integrations/
operations/
system/
```

Traceability documents must reference those definitions rather than restating them.

A missing relationship should remain visible as a gap instead of being filled with an artificial mapping.

---

## Navigation

| Document | Responsibility |
|---|---|
| `traceability-matrix.md` | Current cross-reference matrix, coverage state, and known gaps. |

Future machine-readable traceability metadata may be added alongside the human-readable matrix.

---

## Documentation Rules

All documents in this directory follow:

[`../../rules.md`](../../rules.md)

The main principle is:

> **Traceability connects canonical knowledge; it does not create another copy of it.**
