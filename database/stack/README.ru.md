# Database Stack

> Canonical storage-engine technologies Aveli.

## Current Stack

| Technology | Responsibility |
|---|---|
| [`sqlite/`](sqlite/) | Device-local relational storage engine professional workspace. |
| [`postgresql/`](postgresql/) | Server relational storage engine identity/access/sessions/billing/webhook events. |

## Boundary

`database/stack/` владеет storage-engine knowledge.

Runtime data-access technologies canonical у owning components:

- Drift → [`../../frontend/stack/drift/`](../../frontend/stack/drift/)
- Prisma → [`../../backend/stack/prisma/`](../../backend/stack/prisma/)

Physical schemas:

- [`../local/`](../local/)
- [`../server/`](../server/)
