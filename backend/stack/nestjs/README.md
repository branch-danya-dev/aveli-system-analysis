# NestJS

> Application framework for the Aveli account/access backend.

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

Current modules include `ConfigModule`, `ThrottlerModule`, `PrismaModule`, `HealthModule`, `AuthModule`, `AccessModule`, and `BillingModule`.

## Contextual Usage

- authentication → [`../../auth/`](../../auth/)
- access → [`../../access/`](../../access/)
- billing → [`../../billing/`](../../billing/)
- HTTP contracts → [`../../api/`](../../api/)
- security → [`../../security/`](../../security/)
- persistence access → [`../prisma/`](../prisma/)

## Replaceability

**Medium to low as implementation grows.** Public API/product rules can remain stable, but framework wiring/controllers/guards/filters/tests would need replacement.
