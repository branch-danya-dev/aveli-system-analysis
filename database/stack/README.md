# Database Stack

> Canonical storage-engine technologies used by Aveli.

## Current Stack

| Technology | Responsibility |
|---|---|
| [`sqlite/`](sqlite/) | Device-local relational storage engine for the professional workspace. |
| [`postgresql/`](postgresql/) | Server relational storage engine for identity, access, sessions, billing state, and webhook events. |

## Boundary

`database/stack/` owns **storage-engine knowledge**.

Runtime data-access technologies are canonical with the components that use them:

- Drift → [`../../frontend/stack/drift/`](../../frontend/stack/drift/)
- Prisma → [`../../backend/stack/prisma/`](../../backend/stack/prisma/)

Concrete physical schemas remain owned by:

- [`../local/`](../local/)
- [`../server/`](../server/)

Current rationale is implementation-supported. Historical alternative evaluation should not be presented as fact unless an ADR/equivalent record exists.
