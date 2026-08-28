# Aveli Backend — Access Resolution

> Verified server-side decision model for workspace access.

## Canonical Priority

First valid match wins:

```text
1. lifetime grant
2. manual_temporary grant
3. subscription entitlement support
4. trial grant
5. none
```

## Data Loading

`AccessService` reads:

```text
all access_grants for user
+
subscriptions where entitlement_id = 'support'
```

The pure decision implementation is:

```text
backend/src/access/access.decision.ts
```

## Subscription Entitlement Predicate

A subscription is entitled when:

```text
status ∈ {
  active,
  trialing,
  grace_period,
  past_due,
  cancelled
}
AND
current_period_end > now
```

Non-entitled statuses:

```text
expired
revoked
```

Important consequence:

```text
cancelled + future current_period_end
→ access remains valid until period end
```

## AccessStatusView

Canonical public contract:

[`../api/access/`](../api/access/)

It returns:

```text
hasAccess
source
trialEndsAt
accessEndsAt
subscription
reason
requiresOnlineVerification
verifiedAt
nextVerificationRequiredAt
```

## Online Verification Flag

Verified behavior:

| Effective source | `requiresOnlineVerification` |
|---|---:|
| lifetime | false |
| manual | false |
| subscription | true |
| trial | true |
| none | true |

## Denial Reason

When `hasAccess=false`:

| `reason` | Condition |
|---|---|
| `trial_expired` | trial ended and subscription is not entitled |
| `subscription_expired` | subscription row exists but is not entitled |
| `access_required` | otherwise |

## Offline Verification Hint

The server always calculates:

```text
nextVerificationRequiredAt =
  verifiedAt + SUBSCRIPTION_OFFLINE_GRACE_HOURS
```

Current default:

```text
72 hours
```

The value is an environment-controlled policy hint, not a permanent hardcoded entitlement rule.

The Flutter client persists the verified snapshot in secure storage and may honor the returned window offline.

The server remains the online authority.

## Trial

Registration trial remains:

```text
30 days by current product rule
server-owned
one registration trial per account
```

It cannot be reset through reinstall or local database deletion.

## Access Denial Does Not Own Data

```text
hasAccess = false
        ↓
workspace unavailable

NOT

workspace deleted
```

## Related Documentation

- [`../api/access/`](../api/access/)
- [`../billing/`](../billing/)
- [`../../database/server/`](../../database/server/)
