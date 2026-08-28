# Router Redirect Policy

Single `GoRouter.redirect` централизует auth + access navigation.

Он слушает:

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
| `blocked` | `/access-gate`; paywall allowed. |
| `needsNetwork` | `/access-gate`. |
| `loading` | Access-gate при выходе из auth context. |

Access Gate — route-level policy, не duplicated wrapper вокруг каждого feature screen.
