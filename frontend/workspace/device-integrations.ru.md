# Workspace Device / External Integrations

## Contacts

Technology:

```text
flutter_contacts 2.3.1
```

Client запрашивает read permission и импортирует:

```text
name
phone
deviceContactId
```

Duplicates фильтруются по device-contact id или normalized phone.

Client никогда не пишет изменения обратно в device contacts.

## Visit Photos

Technology:

```text
image_picker 1.1.2
```

Selected media копируется в Aveli visit-photo file tree и referenced из Drift metadata.

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

Используется при profile currency change для conversion amounts.

Failure → `ExchangeRateException` и UI fallback/manual guidance.
