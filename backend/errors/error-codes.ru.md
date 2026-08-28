# Backend Error Codes

| Code | Meaning / Current Usage |
|---|---|
| `AUTH_EMAIL_ALREADY_EXISTS` | Registration email уже занят. |
| `AUTH_INVALID_CREDENTIALS` | Unified failed login result. |
| `AUTH_SESSION_EXPIRED` | Refresh token/session invalid, rotated, revoked или expired. |
| `AUTH_USER_DISABLED` | Account disabled/deleted/unavailable для authenticated operation. |
| `AUTH_VALIDATION_FAILED` | DTO или generic validation failure. |
| `AUTH_NOT_IMPLEMENTED` | Declared auth stub не реализован. |
| `BILLING_SYNC_FAILED` | RevenueCat state нельзя безопасно reconcile при sync. |
| `WEBHOOK_AUTH_REQUIRED` | Required RevenueCat webhook auth configuration отсутствует. |
| `ACCESS_*` | Reserved namespace; access denial сейчас использует `AccessStatusView.reason`. |
| `SUBSCRIPTION_*` | Reserved namespace. |

## Internal Error

Unexpected server failure:

```json
{
  "code": "INTERNAL_ERROR",
  "message": "..."
}
```

## Important Product Boundary

Workspace access denial обычно не является `ACCESS_*` HTTP error.

Canonical denial: HTTP 200 `AccessStatusView`.
