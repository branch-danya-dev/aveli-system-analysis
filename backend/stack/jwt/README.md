# JWT

> Access-token technology used for authenticated Aveli backend requests.

## Role

JWT is the short-lived bearer credential format for authenticated API requests. It carries authenticated identity context across the HTTP boundary.

JWT does **not** represent workspace entitlement:

```text
JWT authentication
      ↓
Authenticated account
      ↓
Access resolution
      ↓
Granted / Denied
```

## Why It Fits

The backend is a narrow stateless HTTP API. JWT allows request authentication without a dedicated access-token row for each request, while current account status is still checked server-side.

## Aveli-Specific Usage

Concrete Aveli claims, TTL, refresh-token format, rotation and reuse behavior are contextual authentication details and are canonical in:

[`../../auth/session-lifecycle.md`](../../auth/session-lifecycle.md)

## Dependencies

- `@nestjs/jwt`
- `passport-jwt`
- `JwtStrategy`
- `JWT_ACCESS_SECRET`
- current account-state validation

## Limitations

Self-contained access tokens may remain cryptographically valid until expiry unless an additional revocation mechanism exists. Signing-secret handling is security-critical, and token claims become part of the internal authentication contract.

The current documentation does not claim a server-side access-token denylist.

## Replaceability

**Medium.** Replacing JWT would affect NestJS guards/strategy, Flutter bearer authentication, token issuance, security configuration and tests, but should not require changing product access rules if authentication and access remain separated.

## Alternatives

Relevant alternatives include opaque server-side access tokens, cookie-backed web sessions for browser-oriented clients, or another signed-token format. No historical ADR records a formal comparison.
