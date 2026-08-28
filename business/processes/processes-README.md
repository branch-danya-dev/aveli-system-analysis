# Processes

> Business-level user journeys and behavioral flows for Aveli.

---

## Purpose

The `processes/` directory describes **how users move through Aveli and how product-level states change over time**.

Process documents focus on user intent, observable behavior, decision points, and business outcomes. They do not describe internal service calls, database operations, SDK behavior, or other implementation details.

---

## Responsibility

This area is responsible for:

- end-to-end user journeys;
- important product decision flows;
- transitions between major user states;
- business-significant lifecycle flows;
- diagrams that clarify user or product behavior.

Technical sequences belong to the component that owns them.

---

## Boundaries

A business process may describe:

```text
User registers
    ↓
Trial becomes available
    ↓
Workspace opens
```

It should not describe:

```text
Backend endpoint called
    ↓
Database row inserted
    ↓
SDK returns entitlement
```

When technical behavior matters, the process should reference the corresponding technical documentation instead of duplicating it.

---

## Navigation

| Document | Responsibility |
|---|---|
| `main-user-journey.md` | Primary journey from product entry to everyday professional work. |
| `access-journey.md` | Product-level journey that determines whether the workspace may be opened. |
| `diagrams/` | Source diagrams supporting the documented journeys. |

Related technical flows are documented in `../../backend/`, `../../frontend/`, `../../integrations/`, and `../../system/`.

---

## Documentation Rules

All process documents follow the repository-wide rules defined in:

[`../../rules.md`](../../rules.md)

For this directory, the main rule is:

> **Describe the user's journey and product decisions here; describe component execution in the technical layer.**
