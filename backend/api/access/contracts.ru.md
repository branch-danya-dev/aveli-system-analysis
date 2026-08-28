# `GET /v1/access`

> Canonical client entitlement endpoint.

## Request

Auth:

```http
Authorization: Bearer <accessToken>
```

Body: none.

Success:

```text
200 OK
```

## Response — `AccessStatusView`

```json
{
  "hasAccess": true,
  "source": "subscription",
  "trialEndsAt": null,
  "accessEndsAt": "2026-09-28T00:00:00.000Z",
  "subscription": {
    "plan": "product-or-entitlement-id",
    "status": "active",
    "autoRenew": true,
    "expiresAt": "2026-09-28T00:00:00.000Z"
  },
  "reason": null,
  "requiresOnlineVerification": true,
  "verifiedAt": "2026-08-28T09:00:00.000Z",
  "nextVerificationRequiredAt": "2026-08-31T09:00:00.000Z"
}
```

## Field Contract

| Field | Type | Meaning |
|---|---|---|
| `hasAccess` | boolean | Effective workspace access decision. |
| `source` | enum | `lifetime`, `manual`, `subscription`, `trial`, `none`. |
| `trialEndsAt` | ISO 8601 UTC or null | Конец registration trial. |
| `accessEndsAt` | ISO 8601 UTC or null | Конец manual/subscription/trial window; null для lifetime. |
| `subscription` | object or null | Normalized current subscription view. |
| `subscription.plan` | string | `productId ?? entitlementId`. |
| `subscription.status` | string | Backend `SubscriptionStatus`. |
| `subscription.autoRenew` | boolean or null | Provider-backed auto-renew state. |
| `subscription.expiresAt` | ISO 8601 UTC or null | Current subscription period end. |
| `reason` | string or null | Denial reason при отсутствии access. |
| `requiresOnlineVerification` | boolean | Нужна ли periodic online verification. |
| `verifiedAt` | ISO 8601 UTC | Время расчета server. |
| `nextVerificationRequiredAt` | ISO 8601 UTC | Server-provided deadline следующей verification. |

## Denial Reasons

```text
trial_expired
subscription_expired
access_required
```

## Product Denial

No entitlement — valid HTTP 200 product state.

Это не 403.

## Flutter Mirror

```text
lib/features/subscription/domain/entities/access_state.dart
AccessState.fromJson
```

Изменение JSON shape является client-breaking contract change.
