# Access Verification Policy

## Purpose

Policy решает, может ли cached entitled access state временно разрешить offline workspace entry.

## Server Deadline Preferred

При наличии:

```text
nextVerificationRequiredAt
```

используется как preferred verification deadline.

## Client Default

Current client также содержит default offline grace:

```text
Duration(hours: 72)
```

в `AccessVerificationPolicy`.

Это **verified client implementation default**.

Его нельзя путать с immutable product rule: backend также возвращает next verification deadline.

## Decision Boundary

Offline grace применяется когда:

```text
hasAccess = true
AND
requiresOnlineVerification = true
```

## Expired Verification

Entitled snapshot с прошедшим deadline дает:

```text
needsNetwork
```

а не local-data deletion.

## Snapshot Cleanup

Controller может очищать expired/denied cached state с disk, когда fallback больше не valid.
