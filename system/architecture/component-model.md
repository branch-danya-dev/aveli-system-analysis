# Aveli — Component Model

## System Components

### Aveli Mobile Client

Primary user-facing component.

Responsibilities:

```text
UI / navigation
local workspace behavior
local persistence usage
session credential handling
access presentation / gating
purchase / restore initiation
device integrations
offline operation
```

Canonical documentation:

[`../../frontend/`](../../frontend/)

### Aveli Backend

Narrow account/access service.

Responsibilities:

```text
registration / authentication
refresh sessions
account lifecycle
trial / grants
subscription normalization
effective access resolution
billing reconciliation
RevenueCat webhook processing
```

It does not own professional workspace records.

Canonical documentation:

[`../../backend/`](../../backend/)

### Local Workspace Persistence

Per-user device persistence:

```text
aveli_<userId>.sqlite
+
visit-photo files
+
secure session/access state
```

SQLite/files are the current source of truth for professional workspace data.

Secure storage holds authentication/access client state outside the workspace database.

Canonical documentation:

[`../../database/local/`](../../database/local/)

### Server Persistence

PostgreSQL stores:

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

Canonical documentation:

[`../../database/server/`](../../database/server/)

### RevenueCat

External subscription abstraction.

Responsibilities for Aveli:

```text
offerings
mobile purchase / restore
provider customer state
server subscriber evidence
webhook events
```

RevenueCat does not directly decide Aveli workspace access.

Canonical documentation:

[`../../integrations/revenuecat/`](../../integrations/revenuecat/)

### Mobile Stores

Apple App Store / Google Play own store-side billing lifecycle.

Aveli reaches them primarily through RevenueCat.

Canonical documentation:

- [`../../integrations/app-store/`](../../integrations/app-store/)
- [`../../integrations/google-play/`](../../integrations/google-play/)

### Device / External Services

Supporting boundaries:

```text
contacts
local notifications
camera/gallery
exchange rate API
share/file picker/SMS/browser
```

Canonical documentation:

[`../../integrations/`](../../integrations/)

## Component Relationship Summary

```text
User
 ↓
Mobile
 ├── Local SQLite / Files
 ├── Secure Storage
 ├── OS / Device Integrations
 ├── RevenueCat SDK
 └── Backend API
        ├── PostgreSQL
        └── RevenueCat REST / Webhook
               ↕
        Apple / Google Stores
```

## Dependency Direction

The professional workspace can operate locally.

Account/access flows depend on backend authority periodically.

Billing depends on external provider/store evidence.

This means Aveli intentionally has different availability characteristics for:

```text
daily workspace
vs
identity/access/billing
```
