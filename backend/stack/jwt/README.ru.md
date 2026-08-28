# JWT

> Access-token technology для authenticated Aveli backend requests.

## Role

JWT используется как short-lived bearer credential format для authenticated API requests и переносит authenticated identity context через HTTP boundary.

JWT **не** представляет workspace entitlement:

```text
JWT authentication
      ↓
Authenticated account
      ↓
Access resolution
      ↓
Granted / Denied
```

## Почему подходит

Backend — узкий stateless HTTP API. JWT позволяет аутентифицировать request без отдельной access-token row на каждый запрос, при этом current account status дополнительно проверяется server-side.

## Aveli-Specific Usage

Concrete Aveli claims, TTL, refresh-token format, rotation и reuse behavior являются contextual authentication details и canonical в:

[`../../auth/session-lifecycle.ru.md`](../../auth/session-lifecycle.ru.md)

## Dependencies

- `@nestjs/jwt`
- `passport-jwt`
- `JwtStrategy`
- `JWT_ACCESS_SECRET`
- current account-state validation

## Limitations

Self-contained access token может оставаться cryptographically valid до expiry, если нет дополнительной revocation logic. Signing-secret handling security-critical, а claims становятся частью internal authentication contract.

Current documentation не утверждает наличие server-side access-token denylist.

## Replaceability

**Medium.** Замена JWT затронет NestJS guards/strategy, Flutter bearer authentication, token issuance, security configuration и tests, но не должна менять product access rules при сохранении separation authentication/access.

## Alternatives

Релевантные alternatives: opaque server-side access tokens, cookie-backed web sessions для browser-oriented clients и другой signed-token format. Historical ADR с formal comparison отсутствует.
