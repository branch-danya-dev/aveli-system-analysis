# Backend Stack

> Canonical technology knowledge Aveli backend.

## Current Runtime Stack

| Technology | Verified Role |
|---|---|
| NestJS 11.x | HTTP/module/service application framework. |
| REST/JSON | Public HTTP contract style. |
| `@nestjs/jwt` + `passport-jwt` | JWT access-token authentication. |
| Argon2id (`argon2` 0.45) | Password hashing. |
| Prisma 6.19 | Backend data access и migrations. |
| PostgreSQL | Backend physical persistence. |
| `class-validator` | DTO validation. |
| `@nestjs/throttler` | Global и per-route rate limiting. |
| `helmet` | HTTP security headers. |

Persistence technology canonical в:

- [`../../database/stack/prisma/`](../../database/stack/prisma/)
- [`../../database/stack/postgresql/`](../../database/stack/postgresql/)

## Canonical Technology Documents

- [`nestjs/`](nestjs/)
- [`rest-api/`](rest-api/)
- [`jwt/`](jwt/)
- [`argon2id/`](argon2id/)

Supporting libraries документируются contextually там, где их behavior важен, без обязательного отдельного stack directory.

## Selection History

Implementation подтверждает current stack.

Historical alternative evaluation не считается зафиксированной без отдельного ADR.
