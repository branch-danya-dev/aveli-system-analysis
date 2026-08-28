# NestJS

> Application framework Aveli account/access backend.

## Role

```text
HTTP ingress
   ↓
controllers
   ↓
services / guards / filters
   ↓
persistence + integrations
```

Current modules: `ConfigModule`, `ThrottlerModule`, `PrismaModule`, `HealthModule`, `AuthModule`, `AccessModule`, `BillingModule`.

Persistence access → [`../prisma/`](../prisma/)

**Replaceability: medium to low as implementation grows.**
