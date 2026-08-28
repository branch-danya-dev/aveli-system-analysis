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

Это mirror backend `AccessStatusView`.

## API Sources

```text
GET  /v1/access
POST /v1/billing/sync
```

## Controller

`AccessController`:

- hydrates secure snapshot on build;
- fetches server access;
- пишет successful server state в snapshot;
- может сохранить allowed cached state при network failure, если policy разрешает;
- syncs billing после purchase/restore;
- refreshes customer/access state on app resume.

## Authority Boundary

Frontend не рассчитывает backend priority между lifetime/manual/subscription/trial.

Он доверяет:

```text
AccessState.hasAccess
```

из server или previously verified secure snapshot.
