# Traceability

> Relationships between business intent, requirements, verification, and technical ownership.

---

## Purpose

The `traceability/` directory connects **canonical knowledge across abstraction levels**.

It allows an important product decision to be followed from business rule or requirement toward verification and technical ownership.

---

## Responsibility

This area is responsible for:

- requirement relationships;
- coverage visibility;
- cross-layer ownership links;
- known traceability gaps;
- forward and backward navigation between business and technical knowledge.

---

## Boundaries

Traceability does not redefine requirements, architecture, or implementation.

Canonical definitions remain in their owning documents.

This area records relationships between them and makes missing relationships visible.

---

## Navigation

| Document | Responsibility |
|---|---|
| `traceability-matrix.md` | Current cross-reference matrix, coverage state, and known gaps. |

Related requirements:

[`../requirements/`](../requirements/)

Related business processes:

[`../processes/`](../processes/)

---

## Documentation Rules

Repository-wide rules:

[`../../rules.md`](../../rules.md)

Core principle:

> **Traceability connects canonical knowledge; it does not create another copy of it.**
