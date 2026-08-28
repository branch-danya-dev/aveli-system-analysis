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

Canonical:

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

Professional workspace records backend не owns.

Canonical:

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

SQLite/files — current source of truth professional workspace data.

Secure storage хранит authentication/access client state отдельно от workspace DB.

Canonical:

[`../../database/local/`](../../database/local/)

### Server Persistence

PostgreSQL хранит:

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

Canonical:

[`../../database/server/`](../../database/server/)

### RevenueCat

External subscription abstraction.

Responsibilities:

```text
offerings
mobile purchase / restore
provider customer state
server subscriber evidence
webhook events
```

RevenueCat не решает Aveli workspace access напрямую.

Canonical:

[`../../integrations/revenuecat/`](../../integrations/revenuecat/)

### Mobile Stores

Apple App Store / Google Play владеют store-side billing lifecycle.

Aveli взаимодействует с ними прежде всего через RevenueCat.

Canonical:

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

Canonical:

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

Professional workspace может работать локально.

Account/access flows периодически зависят от backend authority.

Billing зависит от external provider/store evidence.

Поэтому Aveli имеет разные availability characteristics для:

```text
daily workspace
vs
identity/access/billing
```
