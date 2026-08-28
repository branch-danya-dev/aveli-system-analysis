# REST API

> HTTP interface style Aveli backend.

## Role

REST/JSON — external interface style для authentication, account self-service, access state, billing reconciliation, webhook ingress и health/readiness.

## Почему подходит

Public surface небольшой и request/response oriented, потребляется в основном Flutter плюс RevenueCat webhooks. REST дает conventional HTTP semantics, простые JSON contracts, удобное mobile-client consumption и прямое OpenAPI representation.

## Canonical vs Contextual Knowledge

Этот документ владеет general REST interface decision. Concrete paths, bodies, statuses, errors и authentication requirements canonical в [`../../api/`](../../api/).

## Limitations

REST contracts требуют deliberate evolution paths, HTTP semantics и JSON shapes. `AuthTokensResponse` и `AccessStatusView` client-dependent, поэтому careless changes являются breaking.

## Replaceability

**Low to medium пока current client зависит от REST.** Новый primary interface style потребует coordinated changes во Flutter remote data sources, authentication transport, billing/access calls, tests и API documentation.

## Alternatives

Potential alternatives: GraphQL, gRPC для controlled service-to-service interfaces или другой RPC-style HTTP contract. Historical ADR с formal evaluation отсутствует.
