# Scope

> Product boundary, included capabilities, exclusions, and constraints of Aveli.

---

## Purpose

The `scope/` directory defines **what belongs to Aveli and what does not**.

It establishes the product boundary before deeper functional and technical decomposition.

---

## Responsibility

This area is responsible for:

- in-scope product capabilities;
- out-of-scope capabilities;
- product-level data boundaries;
- supported operating conditions;
- external capability boundaries;
- constraints that may change the shape of the system.

---

## Boundaries

Scope defines **responsibility**, not implementation.

It may state that a capability exists or is excluded, but should not describe how backend, frontend, database, or integrations implement that decision.

Implementation consequences are documented in the corresponding technical areas.

---

## Navigation

| Document | Responsibility |
|---|---|
| `scope.md` | Canonical product scope and system boundary. |
| `diagrams/product-boundary.puml` | Visual representation of the product boundary. |

Continue with:

[`../requirements/`](../requirements/)

---

## Documentation Rules

Repository-wide rules:

[`../../rules.md`](../../rules.md)
