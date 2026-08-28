# Business

> Canonical business-level documentation for Aveli: product context, scope, requirements, processes, acceptance, and traceability.

## Status

**Baseline: Stable**

The business baseline is aligned with the current product boundary and implementation-backed system model. Future product choices are explicitly classified in [`../system/review/open-questions.md`](../system/review/open-questions.md).

## Purpose

`business/` explains **what Aveli is, why it exists, what behavior the system must provide, and which product rules and boundaries must remain true**.

Business documentation owns intent and observable behavior. Technical implementation remains canonical in:

```text
database/
backend/
frontend/
integrations/
system/
```

## Responsibility

This area owns:

- product context and goals;
- system/product scope;
- business rules;
- functional and non-functional requirements;
- acceptance criteria;
- user/product processes;
- traceability from business intent to technical ownership and verification.

## Boundary

Business documentation answers:

```text
Why does the product exist?
Who uses it?
What is in / out of scope?
What behavior is required?
Which rules must remain true?
What result is acceptable?
```

It does not own concrete database schemas, framework choices, API payload internals, or deployment configuration unless a technical constraint itself changes product behavior or an external contract.

## Structure

| Area | Responsibility |
|---|---|
| `context/` | Product background, users, goals, positioning. |
| `scope/` | Product/system boundary and exclusions. |
| `requirements/` | Rules, FR, NFR, acceptance criteria. |
| `processes/` | User/product workflows. |
| `traceability/` | Business → verification → technical ownership relationships. |
| `diagrams/` | Business knowledge maps. |

Business map: [`diagrams/business-map.puml`](diagrams/business-map.puml)

## Reading Path

```text
context/
  ↓
scope/
  ↓
requirements/
  ↓
processes/
  ↓
traceability/
```

For implementation, continue to [`../database/`](../database/), [`../backend/`](../backend/), [`../frontend/`](../frontend/), [`../integrations/`](../integrations/), and [`../system/`](../system/).

## Documentation Rules

[`../rules.md`](../rules.md)
