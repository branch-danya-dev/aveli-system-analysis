# Riverpod

> State-management and dependency-injection technology used throughout Aveli frontend.

## Verified Version

`flutter_riverpod 2.6.1`

## Role

Riverpod provides long-lived infrastructure providers, async controllers, stream/future state and dependency wiring between features and shared services.

A manually created `ProviderContainer` is mounted through `UncontrolledProviderScope`.

Most repository providers are long-lived; `paywallOfferingsProvider` is a verified `FutureProvider.autoDispose` example.

## Replaceability

**Medium.** Providers pervade application composition, but repositories/domain logic provide boundaries that reduce full rewrite scope.
