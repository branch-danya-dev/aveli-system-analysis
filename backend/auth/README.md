# Backend Authentication

> Server-side identity and authenticated-session lifecycle.

## Purpose

`auth/` documents how the backend establishes **who the account is**.

Authentication is intentionally separate from access resolution.

```text
Authentication
    ↓
Who is the user?

Access
    ↓
May the user open the workspace?
```

## Responsibility

This area owns backend behavior for:

- account registration;
- credential validation;
- sign-in;
- authenticated session creation;
- access-token issuance;
- refresh-token rotation;
- session expiration/revocation;
- logout/revocation semantics.

## Navigation

- [`authentication.md`](authentication.md)
- [`session-lifecycle.md`](session-lifecycle.md)
- [`auth-flow.puml`](auth-flow.puml)

Access decision:

[`../access/`](../access/)
