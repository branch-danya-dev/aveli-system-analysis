# Database Implementation Verification

> Audit record между logical data architecture и verified persistence evidence.

## Статус

**Applied — Database baseline Stable**

## Final Classification

| Concept | Classification |
|---|---|
| `confirmed` appointment status | Implementation-supported state; не отдельное product rule. |
| Service `returnInterval` | Optional supported metadata; не core scheduling invariant. |
| Public slug/listing | Legacy/future-facing local capability; не current public-booking contract. |
| Local phone/email verification flags | Local/future support state; real verification не implemented end-to-end. |
| Local account-lifecycle setting | Client-local support state; backend authority unchanged. |
| Exchange-rate cache | Implementation support state. |
| Backend `disabled` / `deleted` | Backend lifecycle/security state. |
| Provider lifecycle states | Integration/backend normalization support. |

Unclassified blockers отсутствуют.

## Known Naming Evidence Gap

```text
ER overview:      duration_minutes / return_interval_minutes
Detailed table:   duration / return_interval
```

Physical docs следуют detailed definition. Это accepted naming-evidence limitation, не data-meaning ambiguity.

## Canonical Results

- [`models/logical/data-model.ru.md`](models/logical/data-model.ru.md)
- [`local/`](local/)
- [`server/`](server/)
