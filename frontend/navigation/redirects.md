# Router Redirect Policy

A single `GoRouter.redirect` callback centralizes auth + access navigation.

It listens to:

```text
authControllerProvider
accessControllerProvider
```

## Decision Summary

```text
standalone mode
→ skip auth/access gate → workspace

unsigned
→ /welcome
  except public auth routes

signed in
→ resolveAccessGateDecision
```

Access decisions:

| Decision | Router behavior |
|---|---|
| `allowed` | Workspace; redirect away from auth/access-gate. |
| `blocked` | `/access-gate`; paywall remains allowed. |
| `needsNetwork` | `/access-gate`. |
| `loading` | Access-gate when leaving auth context. |

Access Gate is route-level policy, not a wrapper duplicated around each feature screen.
