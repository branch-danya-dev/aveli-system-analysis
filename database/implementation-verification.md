# Database Implementation Verification

> Audit record between logical data architecture and verified persistence evidence.

## Status

**Applied — Database baseline Stable**

Verified implementation facts are incorporated into canonical database documentation.

## Final Classification of Previously Ambiguous Implementation Concepts

| Concept | Classification |
|---|---|
| `confirmed` appointment status | Implementation-supported state; not a separate product rule unless UX promotes it. |
| Service `returnInterval` | Optional supported service metadata; not a core scheduling invariant. |
| Public slug/listing values | Legacy/future-facing local capability; not a current public-booking contract. |
| Local phone/email verification flags | Local/future support state; real verification is not implemented end-to-end. |
| Local account-lifecycle setting | Client-local support state; backend account state remains authoritative. |
| Exchange-rate cache | Implementation support state. |
| Backend `disabled` / `deleted` | Backend lifecycle/security states; `deleted` participates explicit profile deletion. |
| Provider subscription lifecycle states | Integration/backend normalization support used to derive access. |

No item above remains an unclassified blocker.

## Known Naming Evidence Gap

```text
ER overview:      duration_minutes / return_interval_minutes
Detailed table:   duration / return_interval
```

Current physical docs follow the detailed definition. This is an accepted naming-evidence limitation, not an architecture/data-meaning ambiguity.

## Canonical Results

- [`models/logical/data-model.md`](models/logical/data-model.md)
- [`local/`](local/)
- [`server/`](server/)
