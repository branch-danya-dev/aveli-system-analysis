# Backend Error Codes

| Code | Meaning / Current Usage |
|---|---|
| `AUTH_EMAIL_ALREADY_EXISTS` | Registration email is already occupied. |
| `AUTH_INVALID_CREDENTIALS` | Unified failed login result. |
| `AUTH_SESSION_EXPIRED` | Refresh token/session invalid, rotated, revoked, or expired. |
| `AUTH_USER_DISABLED` | Account is disabled/deleted/unavailable for authenticated operation. |
| `AUTH_VALIDATION_FAILED` | DTO or generic validation failure. |
| `AUTH_NOT_IMPLEMENTED` | Declared auth stub is not implemented. |
| `BILLING_SYNC_FAILED` | RevenueCat state cannot be safely reconciled during sync. |
| `WEBHOOK_AUTH_REQUIRED` | Required RevenueCat webhook authentication configuration is missing. |
| `ACCESS_*` | Reserved namespace; access denial currently uses `AccessStatusView.reason`. |
| `SUBSCRIPTION_*` | Reserved namespace. |

## Internal Error

Unexpected server failures serialize as:

```json
{
  "code": "INTERNAL_ERROR",
  "message": "..."
}
```

## Important Product Boundary

Workspace access denial is normally not represented by an `ACCESS_*` HTTP error.

Canonical access denial uses HTTP 200 `AccessStatusView`.
