# Secure Client-Side Persistence

> Data persisted locally outside SQLite in secure storage.

This area is documented here because it participates in the overall data-persistence model. The implementation mechanism itself should later be cross-linked to the canonical frontend/security documentation.

## Access Snapshot

Key:

```text
aveli_access_snapshot_<sanitizedUserId>
```

Value: serialized `AccessState` JSON.

Known fields include:

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

The snapshot is used for offline-grace behavior after successful access verification.

It is cleared on logout / delete profile according to the supplied persistence description.

## Auth Tokens

Access and refresh JWT values are also stored through secure client-side storage and are separate from Drift/SQLite.

They participate in selecting the authenticated `userId`, which in turn selects the per-user SQLite database.
