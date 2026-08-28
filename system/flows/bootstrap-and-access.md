# Bootstrap and Workspace Access

## Purpose

This flow explains how an existing user reaches the professional workspace after application start.

## End-to-End Flow

```text
Application start
    ↓
Flutter bootstrap
    ↓
restore authenticated session
    ↓
open local DB for authenticated user
    ↓
RevenueCat logIn(userId)
    ↓
restore local UI preferences
    ↓
restore / verify access state
    ↓
Access Gate decision
    ↓
Workspace OR Access UI
```

## Session Restoration

The mobile client uses the stored refresh credential to restore backend identity.

Canonical frontend behavior:

[`../../frontend/auth/session-lifecycle.md`](../../frontend/auth/session-lifecycle.md)

Canonical backend behavior:

[`../../backend/auth/session-lifecycle.md`](../../backend/auth/session-lifecycle.md)

## Workspace Activation

Authenticated server user id selects:

```text
aveli_<userId>.sqlite
```

and the matching local file/access context.

The local workspace is opened **before** it becomes visible to the user.

## Access Verification

Online:

```text
GET /v1/access
    ↓
Backend access resolution
    ↓
AccessStatusView
    ↓
secure snapshot
```

Server priority:

```text
lifetime
→ manual
→ subscription
→ trial
→ none
```

Canonical backend decision:

[`../../backend/access/access-resolution.md`](../../backend/access/access-resolution.md)

## Final Gate

The frontend does not calculate entitlement precedence.

It evaluates the trusted `AccessState` and verification policy.

Possible outcomes:

```text
allowed
blocked
needsNetwork
loading
```

Canonical frontend gate:

[`../../frontend/access/`](../../frontend/access/)

## Key Invariant

```text
Access denied
≠
workspace deleted
```
