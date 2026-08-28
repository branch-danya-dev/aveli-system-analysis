# Access Gate

## Decision Function

Canonical client decision:

```text
resolveAccessGateDecision
```

## Outcomes

| Decision | Meaning |
|---|---|
| `loading` | No usable access value yet. |
| `allowed` | Workspace may open. |
| `blocked` | `hasAccess == false`. |
| `needsNetwork` | Entitled cached state exists but verification window expired. |

## UI

`AccessGateScreen` supports:

- expired-trial/access messaging;
- continue to paywall;
- restore purchases;
- retry verification;
- sign out.

## Navigation Enforcement

Access Gate is enforced by the global router redirect.

It is **not** duplicated as a wrapper around every feature screen.

## Purchase Boundary

RevenueCat `CustomerInfo` can describe an active entitlement after purchase, but workspace unlock still requires backend `syncBilling` to update `AccessState`.
