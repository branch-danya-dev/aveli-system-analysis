# Secure Client-Side Persistence

> Данные, сохраняемые локально вне SQLite в secure storage.

Область документируется здесь, потому что участвует в общей persistence model. Сам implementation mechanism позже следует cross-link с canonical frontend/security documentation.

## Access Snapshot

Key:

```text
aveli_access_snapshot_<sanitizedUserId>
```

Value: serialized `AccessState` JSON.

Известные поля:

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

Snapshot используется для offline-grace после успешной access verification.

По предоставленному persistence description он очищается при logout / delete profile.

## Auth Tokens

Access и refresh JWT также хранятся через secure client-side storage отдельно от Drift/SQLite.

Они участвуют в определении authenticated `userId`, который затем выбирает per-user SQLite database.
