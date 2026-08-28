# Share / URL / File Picker Boundaries

These integrations are material system boundaries but too small to justify separate top-level provider directories.

## SMS Composer

Technology:

```text
url_launcher 6.3.2
```

Aveli hands off:

```text
phone
message body
```

to the user's SMS application.

Aveli does not send the SMS itself.

## Subscription Management URLs

`url_launcher` opens:

Apple:

```text
https://apps.apple.com/account/subscriptions
```

Google Play:

```text
https://play.google.com/store/account/subscriptions?package=com.aveli.aveli
```

optionally with `sku`.

The store UI remains authoritative for subscription-management actions performed there.

## Share Sheet

Technology:

```text
share_plus 13.3.0
```

Profile export behavior:

```text
generate JSON
  ↓
write temporary file
  ↓
open OS share sheet
  ↓
best-effort delete temp file
```

The user chooses the receiving application.

## File Picker

Technology:

```text
file_picker 12.0.0
```

Profile import:

```text
user selects JSON file
  ↓
Aveli reads UTF-8 content
```

Cancellation returns no selected input rather than a technical failure.

## Boundary Principle

Aveli initiates the handoff, but the target OS application/service owns the interaction after control leaves Aveli.
