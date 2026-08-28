# Aveli — Release Readiness

> Cross-system production-release constraints, сохраненные из legacy security/release documentation.

## Назначение

Документ дает **единый system-level release-readiness view** для mobile client, backend, databases и billing integration.

Он не заменяет component-specific configuration/security documentation.

Canonical implementation owners:

- [`../../frontend/security/`](../../frontend/security/)
- [`../../backend/security/`](../../backend/security/)
- [`../../backend/configuration/`](../../backend/configuration/)
- [`../../database/`](../../database/)
- [`../../integrations/`](../../integrations/)

Задача — убедиться, что individually correct components также aligned как одна production system.

---

## Release Principle

```text
Development Configuration
        ↓
Release Validation
        ↓
Production-Safe Configuration
```

Production build не должен зависеть от development-only behavior.

Unsafe mandatory configuration должна fail до shipping, а не становиться runtime failure у пользователя.

---

## 1. Mobile API Configuration

Production client communication должен использовать valid HTTPS backend endpoint.

Expected:

```text
https://...
```

Release configuration должна reject development endpoints:

```text
http://...
localhost
127.0.0.1
10.0.2.2
```

Production не должен зависеть от emulator/loopback routing.

Canonical:

[`../../frontend/security/`](../../frontend/security/)

---

## 2. Standalone Mode

Development-only standalone behavior:

```text
AVELI_STANDALONE
```

не должен быть enabled в production.

Expected:

```text
Release Build
     ↓
AVELI_STANDALONE = disabled
```

Если standalone enabled для production release, validation должна fail.

---

## 3. Debug Seed

Development/demo seed:

```text
AVELI_DEBUG_SEED
```

не должен silently initialize demo workspace data в production.

Production release config держит debug/demo seed disabled.

---

## 4. RevenueCat Mobile Configuration

Production billing требует correct public SDK configuration target platform.

Current client settings:

```text
REVENUECAT_ANDROID_API_KEY
REVENUECAT_IOS_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Expected entitlement:

```text
support
```

Public RevenueCat mobile keys могут находиться в mobile client.

RevenueCat server secrets — нет.

Canonical:

[`../../integrations/revenuecat/`](../../integrations/revenuecat/)

---

## 5. Backend Secrets

Server-only configuration:

```text
database credentials
JWT signing secrets
RevenueCat server credentials
webhook authentication secrets
other server integration secrets
```

Values должны приходить из backend runtime environment, а не mobile build config или hardcoded public source.

---

## 6. Client Build-Time Configuration

Release-relevant client settings:

| Setting | Purpose |
|---|---|
| `AVELI_API_BASE` | Backend API URL. |
| `REVENUECAT_ANDROID_API_KEY` | Android RevenueCat public SDK key. |
| `REVENUECAT_IOS_API_KEY` | iOS RevenueCat public SDK key. |
| `REVENUECAT_ENTITLEMENT_ID` | Expected RevenueCat entitlement. |
| `AVELI_STANDALONE` | Development-only standalone mode. |
| `AVELI_DEBUG_SEED` | Development/demo seed behavior. |

Production behavior должно определяться explicit production config, а не accidental development defaults.

---

## 7. Release Configuration Gate

Release gate проверяет production-critical conditions:

```text
API uses HTTPS
        +
API is not emulator / loopback
        +
Standalone mode disabled
        +
Required billing configuration present
```

Mandatory release-safety failures должны stop release process.

---

## 8. Validation Pipeline

Legacy release model ожидает automated validation.

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

Exact current CI implementation нужно отдельно verify во время final polish.

Этот документ сохраняет required release behavior, а не утверждает без evidence, что каждая pipeline step уже существует.

---

## 9. Android Release

Production package:

```text
com.aveli.aveli
```

Release signing требует production signing configuration.

Signing credentials должны оставаться вне normal public source control.

Typical local/CI setup может использовать:

```text
key.properties
```

для references на credentials, а не embed secret values в tracked source.

Canonical:

[`../../integrations/google-play/`](../../integrations/google-play/)

---

## 10. iOS Release

Production iOS release требует production-safe:

```text
bundle configuration
signing identity
provisioning configuration
RevenueCat iOS public key
HTTPS API endpoint
```

Development endpoints не должны попадать в production scheme/config.

Canonical:

[`../../integrations/app-store/`](../../integrations/app-store/)

---

## 11. Backend Release Environment

Production backend требует runtime config:

```text
PostgreSQL connection
authentication/JWT secrets
RevenueCat server credentials
webhook authentication
production environment variables
```

Backend не должен зависеть от hardcoded repository credentials.

---

## 12. PostgreSQL / Prisma Migrations

Backend schema changes применяются deliberately во время deployment.

```text
Application Version
       ↓
Required Prisma Migrations
       ↓
Production PostgreSQL
       ↓
Backend Start / Serve
```

Deployed schema должен быть compatible с backend version.

Canonical:

[`../../database/server/migrations/`](../../database/server/migrations/)

---

## 13. Local Drift / SQLite Migrations

Mobile update должен сохранять existing professional workspace data.

Migration safety включает:

```text
clients
appointments
services
payments
visit history
```

Release не должен заставлять user recreate workspace из-за client DB migration.

Canonical:

[`../../database/local/migrations/`](../../database/local/migrations/)

---

## 14. Billing Readiness

Production subscription access зависит от alignment:

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

Mismatch может дать опасный case:

```text
Store purchase succeeds
        ↓
Aveli access not granted
```

Поэтому billing readiness — **cross-system release concern**, а не только mobile/store configuration.

Production store product ids/base plans должны подтверждаться provider-dashboard evidence, а не test fixtures.

---

## 15. Environment Separation

Expected:

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

Development flexibility не должна превращаться в production ambiguity.

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

Перед production release проверить:

- production API endpoint использует HTTPS;
- loopback/emulator API addresses отсутствуют;
- standalone mode disabled;
- debug seed disabled;
- correct platform RevenueCat public config присутствует;
- entitlement mapping aligned с `support`;
- backend secrets server-side only;
- backend runtime environment complete;
- required PostgreSQL/Prisma migrations готовы/применяются deployment process;
- Drift/SQLite migrations preserve existing workspace data;
- Android release signing valid;
- iOS production signing/provisioning valid;
- store products, RevenueCat offerings, backend mapping и client Access Gate aligned;
- required automated validation/tests pass;
- release configuration gate passes.

---

## Final Principle

```text
Development flexibility
        must not become
Production uncertainty
```

Release readiness system-level, потому что identity, billing, access, persistence и client config должны быть согласованы в момент shipping.
