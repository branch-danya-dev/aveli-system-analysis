# Access State

## Model

Canonical client entity:

```text
AccessState
```

Fields:

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

It mirrors the backend `AccessStatusView`.

## API Sources

```text
GET  /v1/access
POST /v1/billing/sync
```

## Controller

`AccessController`:

- hydrates secure snapshot on build;
- fetches server access;
- writes successful server state to snapshot;
- may keep an allowed cached state on network failure if policy permits;
- syncs billing after purchase/restore;
- refreshes customer/access state on app resume.

## Authority Boundary

The frontend does not calculate backend priority between lifetime/manual/subscription/trial.

It trusts:

```text
AccessState.hasAccess
```

from server or previously verified secure snapshot.
