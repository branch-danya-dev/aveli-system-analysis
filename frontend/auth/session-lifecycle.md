# Client Authentication Lifecycle

## Verified Layers

```text
AuthRemoteDataSource
  ↓
AuthRepository / AuthRepositoryImpl
  ↓
AuthController
  ↓
SecureTokenStorage
```

Session domain types:

```text
AuthSession
AuthUser
AuthTokens
```

## Register

```text
POST /v1/auth/register
  ↓
persist returned tokens
  ↓
commit session
  ↓
activate local workspace
```

## Login

```text
POST /v1/auth/login
  ↓
persist returned tokens
  ↓
commit session
  ↓
activate local workspace
```

## Session Restore

Cold-start restore uses the stored refresh token:

```text
read refresh token
  ↓
POST /v1/auth/refresh
  ↓
persist rotated credentials
  ↓
commit session
```

Terminal refresh errors clear stored credentials.

## Access Token Refresh

`AuthRepository.refresh()` is used by access HTTP handling for one retry after a 401.

There is no centralized interceptor middleware.

## Logout

Verified `AuthController.logout` order:

1. cancel all visit reminders;
2. clear access snapshot for current user;
3. RevenueCat `logOut`;
4. backend logout + secure token clear;
5. close current local database;
6. set auth state to `null`.

The SQLite file itself is preserved.

## Delete Profile

Current client delete-profile flow differs from logout:

- calls backend `DELETE /v1/auth/me`;
- deletes visit-photo tree for user;
- deletes local SQLite data for user;
- clears access snapshot;
- clears secure session state.

This is destructive local-data cleanup and must remain distinct from logout behavior.

## Missing Client Capability

Backend supports logout-all, but the Flutter source contains no logout-all client method/UI.

## Auth Stubs

Forgot-password UI calls the backend route, but current backend returns `501 AUTH_NOT_IMPLEMENTED`.

Email verification/password-reset capability is therefore not a shipped end-to-end workflow.
