# Database Stack

> Canonical documentation for persistence technologies used by Aveli.

## Purpose

The `stack/` directory explains **which persistence technologies are used, why they fit the system, where they are used, what depends on them, and what replacing them would affect**.

Technology documentation is canonical here.

Concrete schemas, entities, constraints, and migrations remain owned by:

```text
../local/
../server/
```

## Current Stack

| Technology | Role |
|---|---|
| `SQLite` | Local relational storage engine for the professional workspace. |
| `Drift` | Typed Flutter persistence/data-access layer over SQLite. |
| `PostgreSQL` | Backend relational database for identity, access, billing state, and webhook events. |
| `Prisma` | Schema-first ORM, typed client, and migration layer for the NestJS backend. |

## Navigation

- [`sqlite/`](sqlite/)
- [`drift/`](drift/)
- [`postgresql/`](postgresql/)
- [`prisma/`](prisma/)

## Selection-Rationale Rule

The current implementation provides a strong technical rationale for these choices.

However, the repository does not contain a historical ADR proving that every listed alternative was formally evaluated before implementation.

Therefore stack documentation distinguishes:

```text
Current rationale        → supported by system structure and implementation
Historical decision log  → not formally recorded
```

Rejected alternatives should not be presented as historical fact unless an ADR or equivalent evidence exists.
