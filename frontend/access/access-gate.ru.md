# Access Gate

## Decision Function

Canonical client decision:

```text
resolveAccessGateDecision
```

## Outcomes

| Decision | Meaning |
|---|---|
| `loading` | Пока нет usable access value. |
| `allowed` | Workspace можно открыть. |
| `blocked` | `hasAccess == false`. |
| `needsNetwork` | Entitled cached state есть, но verification window истек. |

## UI

`AccessGateScreen` поддерживает:

- expired-trial/access messaging;
- переход на paywall;
- restore purchases;
- retry verification;
- sign out.

## Navigation Enforcement

Access Gate enforced global router redirect.

Он **не** duplicated wrapper вокруг каждого feature screen.

## Purchase Boundary

RevenueCat `CustomerInfo` может показать active entitlement после purchase, но workspace unlock все равно требует backend `syncBilling`, обновляющий `AccessState`.
