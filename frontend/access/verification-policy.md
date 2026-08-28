# Access Verification Policy

## Purpose

The verification policy decides whether a cached entitled access state may still authorize temporary offline workspace entry.

## Server Deadline Preferred

When present:

```text
nextVerificationRequiredAt
```

is used as the preferred verification deadline.

## Client Default

The current client also contains a default offline grace:

```text
Duration(hours: 72)
```

inside `AccessVerificationPolicy`.

This is a **verified client implementation default**.

It must not be confused with an immutable product rule: the backend also returns the next verification deadline.

## Decision Boundary

Offline grace applies only when:

```text
hasAccess = true
AND
requiresOnlineVerification = true
```

If a source does not require online verification, the snapshot is treated differently according to the policy.

## Expired Verification

An entitled snapshot whose verification deadline has passed produces:

```text
needsNetwork
```

not local-data deletion.

## Snapshot Cleanup

The controller may clear expired/denied cached state from disk when it is no longer valid for fallback.
