# Aveli — Release Readiness

> Cross-system production-release constraints preserved from the legacy security/release documentation.

## Purpose

This document provides a **single system-level release-readiness view** across the mobile client, backend, databases, and billing integration.

It does not replace component-specific configuration/security documentation.

Canonical implementation owners remain:

- [`../../frontend/security/`](../../frontend/security/)
- [`../../backend/security/`](../../backend/security/)
- [`../../backend/configuration/`](../../backend/configuration/)
- [`../../database/`](../../database/)
- [`../../integrations/`](../../integrations/)

The purpose here is to ensure that individually correct components are also aligned as one production system.

---

## Release Principle

```text
Development Configuration
        ↓
Release Validation
        ↓
Production-Safe Configuration
```

A production build must not depend on development-only behavior.

Unsafe mandatory configuration should fail before shipping rather than become a user-visible runtime failure.

---

## 1. Mobile API Configuration

Production client communication must use a valid HTTPS backend endpoint.

Expected:

```text
https://...
```

Release configuration must reject development endpoints such as:

```text
http://...
localhost
127.0.0.1
10.0.2.2
```

Production must not depend on emulator/loopback routing.

Canonical client control:

[`../../frontend/security/`](../../frontend/security/)

---

## 2. Standalone Mode

Development-only standalone behavior:

```text
AVELI_STANDALONE
```

must not be enabled in production.

Expected release condition:

```text
Release Build
     ↓
AVELI_STANDALONE = disabled
```

If standalone mode is enabled for a production release, release validation should fail.

---

## 3. Debug Seed

Development/demo seed behavior:

```text
AVELI_DEBUG_SEED
```

must not silently initialize demo workspace data in production.

Production release configuration must keep debug/demo seed behavior disabled.

---

## 4. RevenueCat Mobile Configuration

Production billing requires the correct public SDK configuration for the target platform.

Current client configuration names include:

```text
REVENUECAT_ANDROID_API_KEY
REVENUECAT_IOS_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Expected entitlement:

```text
support
```

Public RevenueCat mobile keys may exist in the mobile client.

RevenueCat server secrets must not.

Canonical integration:

[`../../integrations/revenuecat/`](../../integrations/revenuecat/)

---

## 5. Backend Secrets

Server-only configuration includes:

```text
database credentials
JWT signing secrets
RevenueCat server credentials
webhook authentication secrets
other server integration secrets
```

These values must come from the backend runtime environment rather than mobile build configuration or hardcoded public source.

---

## 6. Client Build-Time Configuration

Current release-relevant client settings include:

| Setting | Purpose |
|---|---|
| `AVELI_API_BASE` | Backend API URL. |
| `REVENUECAT_ANDROID_API_KEY` | Android RevenueCat public SDK key. |
| `REVENUECAT_IOS_API_KEY` | iOS RevenueCat public SDK key. |
| `REVENUECAT_ENTITLEMENT_ID` | Expected RevenueCat entitlement. |
| `AVELI_STANDALONE` | Development-only standalone mode. |
| `AVELI_DEBUG_SEED` | Development/demo seed behavior. |

Production behavior must be derived from explicit production configuration, not accidental development defaults.

---

## 7. Release Configuration Gate

The release gate should validate production-critical conditions such as:

```text
API uses HTTPS
        +
API is not emulator / loopback
        +
Standalone mode disabled
        +
Required billing configuration present
```

Mandatory release-safety failures should stop the release process.

---

## 8. Validation Pipeline

The legacy release model expects release validation to be part of automated checks.

Conceptually:

```text
Source
  ↓
Analyze
  ↓
Tests
  ↓
Release Configuration Gate
  ↓
Build / Ship
```

The exact current CI implementation should be verified separately during final polish.

This document preserves the required release behavior, not an unverified claim that every pipeline step currently exists.

---

## 9. Android Release

Production Android package:

```text
com.aveli.aveli
```

Release signing requires production signing configuration.

Signing credentials must remain outside normal public source control.

A typical local/CI signing setup may reference credentials through:

```text
key.properties
```

rather than embed secret values directly in tracked source.

Canonical native/store evidence:

[`../../integrations/google-play/`](../../integrations/google-play/)

---

## 10. iOS Release

Production iOS release requires production-safe:

```text
bundle configuration
signing identity
provisioning configuration
RevenueCat iOS public key
HTTPS API endpoint
```

Development endpoints must not leak into the production scheme/configuration.

Canonical native/store evidence:

[`../../integrations/app-store/`](../../integrations/app-store/)

---

## 11. Backend Release Environment

Production backend requires runtime configuration for:

```text
PostgreSQL connection
authentication/JWT secrets
RevenueCat server credentials
webhook authentication
production environment variables
```

The backend must not require hardcoded repository credentials.

---

## 12. PostgreSQL / Prisma Migrations

Backend schema changes must be applied deliberately during deployment.

Conceptual sequence:

```text
Application Version
       ↓
Required Prisma Migrations
       ↓
Production PostgreSQL
       ↓
Backend Start / Serve
```

The deployed backend schema must remain compatible with the backend version being released.

Canonical migration ownership:

[`../../database/server/migrations/`](../../database/server/migrations/)

---

## 13. Local Drift / SQLite Migrations

A mobile update must preserve existing professional workspace data.

Migration safety includes preserving:

```text
clients
appointments
services
payments
visit history
```

A release must not require the user to recreate their workspace because of a client database migration.

Canonical migration ownership:

[`../../database/local/migrations/`](../../database/local/migrations/)

---

## 14. Billing Readiness

Production subscription access depends on alignment across several external/internal layers:

```text
Store Products
      ↓
RevenueCat Offerings
      ↓
support Entitlement
      ↓
Aveli Backend Mapping
      ↓
Client Access Model
```

A mismatch may produce the dangerous case:

```text
Store purchase succeeds
        ↓
Aveli access not granted
```

Therefore billing readiness is a **cross-system release concern**, not only a mobile/store configuration concern.

Production store product ids/base plans remain provider-dashboard evidence and should not be invented from test fixtures.

---

## 15. Environment Separation

Expected separation:

```text
Development
├── emulator/local API
├── debug seed
└── optional standalone

Production
├── HTTPS API
├── real billing configuration
├── production signing
└── no development bypasses
```

Development flexibility must not become production ambiguity.

---

## 16. Failure Strategy

Preferred:

```text
Invalid Production Configuration
            ↓
Validation / Build Failure
```

Avoid:

```text
Invalid Production Configuration
            ↓
Ship
            ↓
User discovers runtime failure
```

---

## Release Checklist

Before shipping a production version, verify:

- production API endpoint uses HTTPS;
- loopback/emulator API addresses are absent;
- standalone mode is disabled;
- debug seed is disabled;
- correct platform RevenueCat public configuration is present;
- entitlement mapping remains aligned with `support`;
- backend secrets remain server-side only;
- backend runtime environment is complete;
- required PostgreSQL/Prisma migrations are ready/applied in the deployment process;
- Drift/SQLite migrations preserve existing workspace data;
- Android release signing configuration is valid;
- iOS production signing/provisioning configuration is valid;
- store products, RevenueCat offerings, backend mapping, and client Access Gate are aligned;
- automated validation/tests required by the release process pass;
- release configuration gate passes.

---

## Final Principle

```text
Development flexibility
        must not become
Production uncertainty
```

Release readiness is system-level because identity, billing, access, persistence, and client configuration must all agree at the moment a version is shipped.
