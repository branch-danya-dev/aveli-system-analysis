# REST API

> HTTP interface style used by the Aveli backend.

## Role

REST/JSON is the external interface style for authentication, account self-service, access state, billing reconciliation, webhook ingress and health/readiness.

## Why It Fits

The public surface is small and request/response oriented, consumed mainly by Flutter plus RevenueCat webhooks. REST provides conventional HTTP semantics, simple JSON contracts, easy mobile-client consumption and direct OpenAPI representation.

## Canonical vs Contextual Knowledge

This document owns the general REST interface decision. Concrete paths, bodies, statuses, errors and authentication requirements are canonical in [`../../api/`](../../api/).

## Limitations

REST contracts require deliberate evolution of paths, HTTP semantics and JSON shapes. `AuthTokensResponse` and `AccessStatusView` are client-dependent shapes, so careless changes are breaking.

## Replaceability

**Low to medium while the current client depends on REST.** A new primary interface style would require coordinated changes across Flutter remote data sources, authentication transport, billing/access calls, tests and API documentation.

## Alternatives

Potential alternatives include GraphQL, gRPC for controlled service-to-service interfaces, or another RPC-style HTTP contract. No historical ADR records formal evaluation.
