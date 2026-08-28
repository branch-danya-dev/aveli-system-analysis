# NestJS

> Application framework for the Aveli account/access backend.

## Role

NestJS provides the application structure around backend capabilities:

```text
HTTP ingress
    ↓
controllers
    ↓
services / guards / filters
    ↓
persistence and integrations
```

Current modules include `ConfigModule`, `ThrottlerModule`, `PrismaModule`, `HealthModule`, `AuthModule`, `AccessModule` and `BillingModule`.

## Why It Fits

Aveli backend is a compact TypeScript service with several clearly separated modules and cross-cutting HTTP concerns. NestJS supports this through explicit modules, dependency injection, controllers, DTO validation, Passport guards, exception filters and testable service boundaries.

## Contextual Usage

- authentication → [`../../auth/`](../../auth/)
- access → [`../../access/`](../../access/)
- billing → [`../../billing/`](../../billing/)
- HTTP contracts → [`../../api/`](../../api/)
- security → [`../../security/`](../../security/)

Persistence is accessed through Prisma: [`../../../database/stack/prisma/`](../../../database/stack/prisma/).

## Limitations

NestJS introduces framework conventions, decorators, DI coupling and Nest-specific guard/filter/controller abstractions.

## Replaceability

**Medium to low as implementation grows.** Public API contracts and product rules can remain stable, but controllers, DI/module wiring, guards, filters, validation integration and framework-level tests would need replacement.

## Alternatives

Comparable choices include Fastify with explicit wiring, Express with custom layering, or another TypeScript server framework. No historical ADR records formal comparison.
