# Backend Access Resolution

> Server-side resolution of whether an authenticated account may open the Aveli workspace.

## Purpose

`access/` owns the deterministic access decision.

Authentication establishes identity.

Access resolution evaluates entitlement state.

## Canonical Priority

```text
Lifetime
   ↓
Manual Grant
   ↓
Active Subscription
   ↓
Active Trial
   ↓
None
```

The first valid source becomes the effective access source.

## Responsibility

This area owns:

- reading valid access sources;
- evaluating source validity;
- deterministic source priority;
- resolving granted/denied workspace access;
- trial/access expiration behavior at the backend decision level.

It does not own local workspace data and does not delete it.

## Navigation

- [`access-resolution.md`](access-resolution.md)
- [`access-resolution.puml`](access-resolution.puml)

Physical access persistence:

[`../../database/server/`](../../database/server/)
