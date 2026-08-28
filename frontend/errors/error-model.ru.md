# Frontend Error Model

## Backend Errors

Auth/access data sources сохраняют backend error code из:

```json
{
  "code": "...",
  "message": "..."
}
```

в:

```text
AuthException.code
```

## Domain Errors

Client также использует typed domain exceptions:

```text
ScheduleConflict
ClientHasUpcomingAppointments
ExchangeRateException
PhotoAccessDenied
```

## UI Mapping

User-facing mapping:

```text
userMessageForError
authErrorMessage
```

с localized strings.

Presentation helpers:

```text
showAveliSnackBar
showAveliError
```

## Logging

Current logging:

```text
logError(operation, error, stackTrace)
→ debugPrint
```

Verified auth path намеренно не логирует tokens.

## Retry Behavior

Centralized retry middleware отсутствует.

Verified special case в access HTTP handling:

```text
401
 ↓
AuthRepository.refresh()
 ↓
retry once
```

Остальной recovery feature-specific.
