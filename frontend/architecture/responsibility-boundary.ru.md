# Aveli Frontend — Responsibility Boundary

## Миссия Frontend

Frontend предоставляет specialist working environment и координирует device-local work с backend-owned identity/access authority.

## Frontend-Owned Capabilities

### Interaction и Navigation

- screens и user interaction;
- navigation и route redirects;
- workspace shell и deep links;
- local presentation state.

### Operational Workspace

Frontend владеет application behavior вокруг device-local:

```text
clients
services
appointments
visit notes/photos
payments
today/calendar
schedule
settings/profile
```

Canonical physical persistence остается в [`../../database/local/`](../../database/local/).

### Session Client

Frontend:

- собирает credentials;
- вызывает `/v1/auth/*`;
- хранит access/refresh credentials в secure storage;
- восстанавливает session через refresh;
- открывает правильную per-user local database;
- выполняет RevenueCat logIn с server UUID;
- делает client cleanup при logout.

Backend остается authoritative для session validity.

### Access Gate

Client consumes server `AccessStatusView`, сохраняет secure snapshot, применяет offline verification policy и маршрутизирует в workspace или access UI.

Client **не** reimplements server priority:

```text
lifetime → manual → subscription → trial
```

Он доверяет `AccessState.hasAccess` от backend/snapshot.

### Billing Client

Frontend использует RevenueCat SDK для purchase/restore UI, затем вызывает backend billing sync.

RevenueCat client result сам по себе не является final workspace-unlock authority.

### Device Integrations

Verified client-side integrations:

- local notifications;
- device contacts read/import;
- local filesystem visit photos;
- connectivity status hint;
- image picker;
- exchange-rate HTTP lookup.

## Explicit Non-Responsibility

Frontend не владеет:

- PostgreSQL account/session records;
- server trial creation;
- server access-grant precedence;
- normalized server subscription authority;
- RevenueCat webhook processing;
- backend secrets;
- cloud sync professional workspace entities.

## Local Isolation

Authenticated server user id определяет local workspace:

```text
users.id
   ↓
aveli_<userId>.sqlite
```

Visit-photo paths используют ту же per-user boundary.

## Important Lifecycle Distinction

### Logout

Logout очищает active client identity/access state и закрывает current DB, но **не** удаляет local workspace database.

### Profile Delete

Account/profile deletion сильнее: current client удаляет user photos, SQLite data, secure snapshot и session state как часть profile-deletion flow.

Эти behaviors нельзя смешивать.

## Current Out-of-Scope / Missing Client Capabilities

- logout-all-devices UI/client method;
- cloud workspace synchronization;
- implemented email verification/password reset backend behavior;
- multi-account UI;
- global RevenueCat customer-info listener;
- verified legacy `aveli.db` claim UI.
