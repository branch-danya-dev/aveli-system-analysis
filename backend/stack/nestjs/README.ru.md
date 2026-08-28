# NestJS

> Application framework Aveli account/access backend.

## Role

NestJS задает application structure backend capabilities:

```text
HTTP ingress
    ↓
controllers
    ↓
services / guards / filters
    ↓
persistence and integrations
```

Current modules: `ConfigModule`, `ThrottlerModule`, `PrismaModule`, `HealthModule`, `AuthModule`, `AccessModule`, `BillingModule`.

## Почему подходит

Aveli backend — компактный TypeScript service с несколькими четко разделенными modules и cross-cutting HTTP concerns. NestJS поддерживает это через explicit modules, dependency injection, controllers, DTO validation, Passport guards, exception filters и testable service boundaries.

## Contextual Usage

- authentication → [`../../auth/`](../../auth/)
- access → [`../../access/`](../../access/)
- billing → [`../../billing/`](../../billing/)
- HTTP contracts → [`../../api/`](../../api/)
- security → [`../../security/`](../../security/)

Persistence идет через Prisma: [`../../../database/stack/prisma/`](../../../database/stack/prisma/).

## Limitations

NestJS вводит framework conventions, decorators, DI coupling и Nest-specific guard/filter/controller abstractions.

## Replaceability

**Medium to low по мере роста implementation.** Public API contracts и product rules могут остаться stable, но controllers, DI/module wiring, guards, filters, validation integration и framework-level tests придется заменить.

## Alternatives

Сопоставимые choices: Fastify с explicit wiring, Express с custom layering или другой TypeScript server framework. Historical ADR с formal comparison отсутствует.
