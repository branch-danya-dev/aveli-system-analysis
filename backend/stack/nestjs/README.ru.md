# NestJS

> Backend application framework Aveli account/access server.

## Verified Version

```text
@nestjs/* 11.x
```

## Role

NestJS размещает backend module structure:

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

`backend/src/main.ts` применяет:

- `helmet`;
- global `ValidationPipe`;
- optional CORS.

Global `ApiExceptionFilter` нормализует HTTP error responses.

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

Дополнительные auth-route limits описаны в backend security/API documentation.

## Persistence Usage

NestJS services используют Prisma.

Canonical Prisma technology documentation:

[`../../../database/stack/prisma/`](../../../database/stack/prisma/)

## Boundary

NestJS — implementation technology, а не product rule или public contract.

Public contract ownership:

[`../../api/`](../../api/)
