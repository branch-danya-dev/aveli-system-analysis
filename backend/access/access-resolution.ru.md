# Aveli Backend — Access Resolution

> Проверенная server-side decision model workspace access.

## Canonical Priority

Первый valid match wins:

```text
1. lifetime grant
2. manual_temporary grant
3. subscription entitlement support
4. trial grant
5. none
```

## Data Loading

`AccessService` читает:

```text
all access_grants for user
+
subscriptions where entitlement_id = 'support'
```

Pure decision implementation:

```text
backend/src/access/access.decision.ts
```

## Subscription Entitlement Predicate

Subscription entitled когда:

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

Важное следствие:

```text
cancelled + future current_period_end
→ access remains valid until period end
```

## AccessStatusView

Canonical public contract:

[`../api/access/`](../api/access/)

Он возвращает:

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

При `hasAccess=false`:

| `reason` | Condition |
|---|---|
| `trial_expired` | trial ended, subscription not entitled |
| `subscription_expired` | subscription row существует, но not entitled |
| `access_required` | иначе |

## Offline Verification Hint

Server всегда вычисляет:

```text
nextVerificationRequiredAt =
  verifiedAt + SUBSCRIPTION_OFFLINE_GRACE_HOURS
```

Current default:

```text
72 hours
```

Это environment-controlled policy hint, а не permanent hardcoded entitlement rule.

Flutter сохраняет verified snapshot в secure storage и может использовать возвращенное окно offline.

Server остается online authority.

## Trial

Registration trial:

```text
30 days по current product rule
server-owned
one registration trial per account
```

Он не сбрасывается reinstall или local database deletion.

## Access Denial Does Not Own Data

```text
hasAccess = false
        ↓
workspace unavailable

NOT

workspace deleted
```

## Связанная документация

- [`../api/access/`](../api/access/)
- [`../billing/`](../billing/)
- [`../../database/server/`](../../database/server/)
