# Backend Stack

> Canonical technology knowledge for the Aveli backend.

## Current Runtime Stack

| Technology | Verified Role |
|---|---|
| NestJS 11.x | HTTP/module/service application framework. |
| REST/JSON | Public HTTP contract style. |
| `@nestjs/jwt` + `passport-jwt` | JWT access-token authentication. |
| Argon2id (`argon2` 0.45) | Password hashing. |
| Prisma 6.19 | Backend data access and migrations. |
| PostgreSQL | Backend physical persistence. |
| `class-validator` | DTO validation. |
| `@nestjs/throttler` | Global and per-route rate limiting. |
| `helmet` | HTTP security headers. |

Persistence technology remains canonical in:

- [`../../database/stack/prisma/`](../../database/stack/prisma/)
- [`../../database/stack/postgresql/`](../../database/stack/postgresql/)

## Canonical Technology Documents

- [`nestjs/`](nestjs/)
- [`rest-api/`](rest-api/)
- [`jwt/`](jwt/)
- [`argon2id/`](argon2id/)

Supporting libraries are documented contextually where their behavior matters rather than receiving separate stack directories by default.

## Selection History

The implementation confirms the current stack.

Historical alternative evaluation is not formally recorded unless a later ADR provides it.
