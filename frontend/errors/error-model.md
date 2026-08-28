# Frontend Error Model

## Backend Errors

Auth/access data sources preserve backend error code from:

```json
{
  "code": "...",
  "message": "..."
}
```

inside:

```text
AuthException.code
```

## Domain Errors

The client also uses typed domain exceptions such as:

```text
ScheduleConflict
ClientHasUpcomingAppointments
ExchangeRateException
PhotoAccessDenied
```

## UI Mapping

User-facing mapping is owned by:

```text
userMessageForError
authErrorMessage
```

with localized strings.

Presentation helpers include:

```text
showAveliSnackBar
showAveliError
```

## Logging

Current error logging uses:

```text
logError(operation, error, stackTrace)
→ debugPrint
```

The verified auth path does not deliberately log tokens.

## Retry Behavior

There is no centralized retry middleware.

A verified special case exists in access HTTP handling:

```text
401
 ↓
AuthRepository.refresh()
 ↓
retry once
```

Other recovery behavior remains feature-specific.
