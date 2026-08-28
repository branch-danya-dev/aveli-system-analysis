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

Cold-start restore использует stored refresh token:

```text
read refresh token
  ↓
POST /v1/auth/refresh
  ↓
persist rotated credentials
  ↓
commit session
```

Terminal refresh errors очищают stored credentials.

## Access Token Refresh

`AuthRepository.refresh()` используется access HTTP handling для one retry после 401.

Centralized interceptor middleware отсутствует.

## Logout

Verified `AuthController.logout` order:

1. cancel all visit reminders;
2. clear access snapshot current user;
3. RevenueCat `logOut`;
4. backend logout + secure token clear;
5. close current local database;
6. set auth state → `null`.

SQLite file сохраняется.

## Delete Profile

Current client delete-profile flow отличается от logout:

- вызывает backend `DELETE /v1/auth/me`;
- удаляет visit-photo tree user;
- удаляет local SQLite data user;
- очищает access snapshot;
- очищает secure session state.

Это destructive local-data cleanup и его нельзя смешивать с logout behavior.

## Missing Client Capability

Backend поддерживает logout-all, но Flutter source не содержит logout-all client method/UI.

## Auth Stubs

Forgot-password UI вызывает backend route, но current backend отвечает `501 AUTH_NOT_IMPLEMENTED`.

Email verification/password-reset не являются shipped end-to-end workflow.
