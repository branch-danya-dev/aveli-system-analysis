# Share / URL / File Picker Boundaries

Эти integrations являются material system boundaries, но слишком малы для отдельных top-level provider directories.

## SMS Composer

Technology:

```text
url_launcher 6.3.2
```

Aveli передает:

```text
phone
message body
```

в user SMS application.

Aveli сам SMS не отправляет.

## Subscription Management URLs

`url_launcher` открывает:

Apple:

```text
https://apps.apple.com/account/subscriptions
```

Google Play:

```text
https://play.google.com/store/account/subscriptions?package=com.aveli.aveli
```

с optional `sku`.

Store UI остается authoritative для subscription-management actions внутри store.

## Share Sheet

Technology:

```text
share_plus 13.3.0
```

Profile export:

```text
generate JSON
  ↓
write temporary file
  ↓
open OS share sheet
  ↓
best-effort delete temp file
```

Target application выбирает user.

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

Cancel дает отсутствие input, а не technical failure.

## Boundary Principle

Aveli initiates handoff, но target OS application/service владеет interaction после передачи control.
