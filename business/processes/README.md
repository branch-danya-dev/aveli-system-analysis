# Processes

> Business-level user journeys and behavioral flows for Aveli.

---

## Purpose

The `processes/` directory describes **how users move through Aveli and how significant product states change over time**.

Processes connect requirements with observable user and product behavior.

---

## Responsibility

This area is responsible for:

- end-to-end user journeys;
- product decision flows;
- business-significant lifecycle transitions;
- flow diagrams that clarify behavior.

---

## Boundaries

Processes describe **user intent, product decisions, and observable transitions**.

They should not describe internal service calls, database operations, SDK behavior, or implementation sequences.

Technical sequences belong to the component that owns them.

---

## Navigation

| Document | Responsibility |
|---|---|
| `main-user-journey.md` | Primary journey from product entry to everyday professional work. |
| `access-journey.md` | Product-level workspace access journey. |
| `diagrams/main-user-flow.puml` | Visual representation of the primary user flow. |
| `diagrams/access-flow.puml` | Visual representation of the access decision flow. |

Related requirements:

[`../requirements/`](../requirements/)

---

## Documentation Rules

Repository-wide rules:

[`../../rules.md`](../../rules.md)
