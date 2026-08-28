# Frontend

> Canonical documentation Aveli Flutter client и local-first professional workspace runtime.

## Статус

**Baseline: Stable**

Frontend baseline reconciled с Flutter `0.2.2+4`, `lib/`, package evidence, navigation/state/data flows и final ownership review.

## Verified Source Shape

```text
lib/
├── app/
├── core/
└── features/
    ├── appointments/
    ├── auth/
    ├── bootstrap/
    ├── calendar/
    ├── clients/
    ├── legal/
    ├── more/
    ├── payments/
    ├── reminders/
    ├── services/
    ├── settings/
    ├── subscription/
    └── today/
```

Client may temporary trust verified snapshot offline, но backend entitlement precedence не пересчитывает.

Verification: [`implementation-verification.ru.md`](implementation-verification.ru.md)

## Documentation Rules

[`../rules.ru.md`](../rules.ru.md)
