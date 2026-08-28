# Requirements

> Business rules, functional requirements, quality expectations, and acceptance criteria for Aveli.

---

## Purpose

The `requirements/` directory defines **what behavior the product must provide, which rules must remain true, and how that behavior is accepted**.

It is the canonical business-level source for requirement identifiers used throughout the repository.

---

## Responsibility

This area is responsible for:

- business rules;
- functional requirements;
- non-functional requirements;
- acceptance criteria;
- stable identifiers used by traceability.

---

## Boundaries

Requirements describe **required and verifiable product behavior**.

They should not define internal implementation mechanisms unless a technical characteristic is itself part of the required external behavior or constraint.

Detailed execution belongs to the technical area that owns it.

---

## Navigation

| Document | Responsibility |
|---|---|
| `business-rules.md` | Product invariants and business constraints. |
| `functional-requirements.md` | Required product capabilities and behavior. |
| `non-functional-requirements.md` | Product-level quality expectations. |
| `acceptance-criteria.md` | Observable conditions used to verify requirements and rules. |

Related behavioral flows:

[`../processes/`](../processes/)

Traceability:

[`../traceability/`](../traceability/)

---

## Documentation Rules

Repository-wide rules:

[`../../rules.md`](../../rules.md)
