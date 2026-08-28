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

The client requests read permission.

## Data Read

Aveli reads:

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

Duplicate detection uses:

- `deviceContactId`;
- normalized phone.

## Write Boundary

The integration is **read-only**.

Aveli does not write modifications back to the device address book.

The imported Aveli `Client` becomes a separate device-local workspace record.

## Permission Denied

When permission is not granted:

```text
listWithPhones → []
```

Import is unavailable, but existing Aveli client data remain untouched.

## Consumer

[`../../frontend/workspace/feature-map.md`](../../frontend/workspace/feature-map.md)
