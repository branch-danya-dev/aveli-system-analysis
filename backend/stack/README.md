# Backend Stack

> Canonical runtime technology knowledge for the Aveli backend.

## Current Stack

| Technology | Role | Canonical |
|---|---|---|
| NestJS 11.x | Application/module/HTTP framework | [`nestjs/`](nestjs/) |
| REST/JSON | HTTP architectural style | [`rest-api/`](rest-api/) |
| JWT + passport-jwt | Access-token authentication | [`jwt/`](jwt/) |
| Argon2id | Password hashing | [`argon2id/`](argon2id/) |
| Prisma 6.19 | Backend data-access/migration technology | [`prisma/`](prisma/) |
| PostgreSQL | Server storage engine | [`../../database/stack/postgresql/`](../../database/stack/postgresql/) |

Supporting libraries such as `class-validator`, `@nestjs/throttler`, and `helmet` are documented contextually.

## Ownership Boundary

```text
Prisma
→ backend runtime data access
→ backend/stack/prisma/

PostgreSQL
→ physical storage engine
→ database/stack/postgresql/
```
