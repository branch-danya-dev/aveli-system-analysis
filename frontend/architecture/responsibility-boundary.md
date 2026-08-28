# Aveli Frontend — Responsibility Boundary

## Frontend Mission

The frontend provides the specialist's working environment and coordinates device-local work with backend-owned identity/access authority.

## Frontend-Owned Capabilities

### Interaction and Navigation

- screens and user interaction;
- navigation and route redirects;
- workspace shell and deep links;
- local presentation state.

### Operational Workspace

The frontend owns application behavior around device-local:

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

Canonical physical persistence remains in [`../../database/local/`](../../database/local/).

### Session Client

The frontend:

- collects credentials;
- calls `/v1/auth/*`;
- persists access/refresh credentials in secure storage;
- restores a session through refresh;
- opens the correct per-user local database;
- logs RevenueCat in with server UUID;
- performs client cleanup on logout.

The backend remains authoritative for session validity.

### Access Gate

The client consumes server `AccessStatusView`, persists a secure snapshot, applies the offline verification policy, and routes to workspace or access UI.

The client does **not** reimplement the server priority:

```text
lifetime → manual → subscription → trial
```

It trusts `AccessState.hasAccess` from the backend/snapshot.

### Billing Client

The frontend uses RevenueCat SDK for purchase/restore UI and then calls backend billing sync.

A RevenueCat client result alone is not the final workspace-unlock authority.

### Device Integrations

Verified client-side integrations include:

- local notifications;
- device contacts read/import;
- local filesystem for visit photos;
- connectivity status hint;
- image picker;
- exchange-rate HTTP lookup.

## Explicit Non-Responsibility

The frontend does not own:

- PostgreSQL account/session records;
- server trial creation;
- server access-grant precedence;
- normalized server subscription authority;
- RevenueCat webhook processing;
- backend secrets;
- cloud sync of professional workspace entities.

## Local Isolation

Authenticated server user id determines the local workspace:

```text
users.id
   ↓
aveli_<userId>.sqlite
```

Visit-photo paths use the same per-user boundary.

## Important Lifecycle Distinction

### Logout

Logout clears active client identity/access state and closes the current database, but does **not** delete the local workspace database.

### Profile Delete

Account/profile deletion is stronger: the current client deletes user photos, SQLite data, secure snapshot, and session state as part of the profile-deletion flow.

These behaviors must not be conflated.

## Current Out-of-Scope / Missing Client Capabilities

- logout-all-devices UI/client method;
- cloud workspace synchronization;
- implemented email verification/password reset backend behavior;
- multi-account UI;
- global RevenueCat customer-info listener;
- verified legacy `aveli.db` claim UI.
