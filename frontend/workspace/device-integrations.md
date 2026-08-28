# Workspace Device / External Integrations

## Contacts

Technology:

```text
flutter_contacts 2.3.1
```

The client requests read permission and imports:

```text
name
phone
deviceContactId
```

Duplicate import is prevented using device-contact id or normalized phone checks.

The client never writes changes back to device contacts.

## Visit Photos

Technology:

```text
image_picker 1.1.2
```

Selected media is copied into the Aveli visit-photo file tree and referenced from Drift metadata.

## Exchange Rate

Repository:

```text
OpenErExchangeRateRepository
```

External API:

```text
https://open.er-api.com/v6/latest/{base}
```

Cache:

```text
app_settings.exchangeRateCacheV1
TTL = 24 hours
```

Used during profile currency changes when amounts need conversion.

Failure is surfaced as `ExchangeRateException` with UI fallback/manual guidance.
