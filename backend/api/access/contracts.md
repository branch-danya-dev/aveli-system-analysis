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
| `trialEndsAt` | ISO 8601 UTC or null | Registration-trial end when relevant. |
| `accessEndsAt` | ISO 8601 UTC or null | End of manual/subscription/trial access window; null for lifetime. |
| `subscription` | object or null | Normalized current subscription view. |
| `subscription.plan` | string | `productId ?? entitlementId`. |
| `subscription.status` | string | Backend `SubscriptionStatus`. |
| `subscription.autoRenew` | boolean or null | Provider-backed auto-renew state. |
| `subscription.expiresAt` | ISO 8601 UTC or null | Current subscription period end. |
| `reason` | string or null | Denial reason when no access. |
| `requiresOnlineVerification` | boolean | Whether the access source requires periodic online verification. |
| `verifiedAt` | ISO 8601 UTC | Server calculation time. |
| `nextVerificationRequiredAt` | ISO 8601 UTC | Server-provided next verification deadline. |

## Denial Reasons

```text
trial_expired
subscription_expired
access_required
```

## Product Denial

No entitlement is a valid HTTP 200 product state.

It is not represented as 403.

## Flutter Mirror

```text
lib/features/subscription/domain/entities/access_state.dart
AccessState.fromJson
```

Changing this JSON shape is a client-breaking contract change.
