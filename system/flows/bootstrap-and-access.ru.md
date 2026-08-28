# Bootstrap and Workspace Access

## Purpose

Flow объясняет, как existing user попадает в professional workspace после application start.

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

Mobile client использует stored refresh credential для restore backend identity.

Frontend:

[`../../frontend/auth/session-lifecycle.ru.md`](../../frontend/auth/session-lifecycle.ru.md)

Backend:

[`../../backend/auth/session-lifecycle.ru.md`](../../backend/auth/session-lifecycle.ru.md)

## Workspace Activation

Authenticated server user id выбирает:

```text
aveli_<userId>.sqlite
```

и matching local file/access context.

Local workspace открывается **до** показа пользователю.

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

Canonical:

[`../../backend/access/access-resolution.ru.md`](../../backend/access/access-resolution.ru.md)

## Final Gate

Frontend не рассчитывает entitlement precedence.

Он evaluates trusted `AccessState` + verification policy.

Outcomes:

```text
allowed
blocked
needsNetwork
loading
```

Canonical:

[`../../frontend/access/`](../../frontend/access/)

## Key Invariant

```text
Access denied
≠
workspace deleted
```
