# NestJS

> Backend application framework used by the Aveli account/access server.

## Verified Version

```text
@nestjs/* 11.x
```

## Role

NestJS hosts the backend module structure:

```text
AppModule
├── ConfigModule
├── ThrottlerModule
├── PrismaModule
├── HealthModule
├── AuthModule
├── AccessModule
└── BillingModule
```

## Bootstrap Behavior

`backend/src/main.ts` applies:

- `helmet`;
- global `ValidationPipe`;
- optional CORS.

A global `ApiExceptionFilter` normalizes HTTP error responses.

## Supporting Framework Behavior

Validation:

```text
whitelist = true
forbidNonWhitelisted = true
transform = true
```

Rate limiting:

```text
global: 120 requests / 60 seconds / IP
```

Additional auth-route limits are documented in backend security/API documentation.

## Persistence Usage

NestJS services use Prisma for backend persistence.

Canonical Prisma technology documentation:

[`../../../database/stack/prisma/`](../../../database/stack/prisma/)

## Boundary

NestJS is implementation technology, not a product rule or public contract.

Public contract ownership remains in:

[`../../api/`](../../api/)
