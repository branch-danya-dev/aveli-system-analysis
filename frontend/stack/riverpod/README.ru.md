# Riverpod

> State-management и dependency-injection technology Aveli frontend.

## Verified Version

`flutter_riverpod 2.6.1`

## Role

Riverpod предоставляет long-lived infrastructure providers, async controllers, stream/future state и dependency wiring между features/shared services.

Manual `ProviderContainer` mounted через `UncontrolledProviderScope`.

Большинство repository providers long-lived; `paywallOfferingsProvider` — verified `FutureProvider.autoDispose` example.

## Replaceability

**Medium.** Providers широко используются, но repository/domain boundaries уменьшают scope потенциальной замены.
