# Device Contacts — Read-Only Import

## Technology

```text
flutter_contacts 2.3.1
```

Repository:

```text
FlutterDeviceContactsRepository
```

## Permissions

Android:

```text
READ_CONTACTS
```

iOS:

```text
NSContactsUsageDescription
```

Client запрашивает read permission.

## Data Read

Aveli читает:

```text
displayName
first non-empty phone
contact.id
```

## Import Processing

```text
Device Contact
  ↓
read fields
  ↓
normalize phone
  ↓
deduplicate
  ↓
create/enrich local Aveli Client
```

Duplicate detection:

- `deviceContactId`;
- normalized phone.

## Write Boundary

Integration **read-only**.

Aveli не пишет изменения обратно в device address book.

Imported Aveli `Client` становится отдельным device-local workspace record.

## Permission Denied

Без permission:

```text
listWithPhones → []
```

Import unavailable, existing Aveli client data остаются unchanged.

## Consumer

[`../../frontend/workspace/feature-map.ru.md`](../../frontend/workspace/feature-map.ru.md)
