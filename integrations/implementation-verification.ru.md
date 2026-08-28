# Integrations Implementation Verification

> Record сверки prior integration documentation с current implementation evidence.

## Статус

**Applied to this integration implementation pass**

Canonical knowledge должно находиться в owning integration directory.

## Verified Baseline

Evidence покрывает:

```text
Flutter 0.2.2+4
backend billing module
native Android manifest/build
native iOS Info.plist/Xcode project
RevenueCat mobile + backend
device integrations
integration tests
```

## Important Corrections Applied

### Internal API Removed from External Integration Ownership

NestJS account API — internal Aveli boundary.

Canonical:

```text
backend/api/
frontend/
```

и не promoted как provider integration.

### RevenueCat Client Does Not Unlock Workspace

Verified flow:

```text
RevenueCat purchase/restore
→ backend billing sync
→ AccessStatusView
→ Access Gate
```

### Store Product IDs Remain OPEN

Имена только из backend test fixtures:

```text
aveli_support_monthly
aveli_support_yearly
```

не документируются как verified production App Store / Play product ids.

### RevenueCat Gateway Has No Explicit Retry/Timeout

Current backend gateway:

```text
retry: none
explicit fetch timeout: none
```

Это теперь visible, а не inferred.

### Webhook Event Type Is Not Access Logic

Webhook triggers RevenueCat REST reconciliation.

Backend не encodes workspace access напрямую из `event_type`.

### Contacts Are Read-Only

Device contacts читаются/imported в Aveli local records.

Write-back path отсутствует.

### Notifications Are Local

FCM/APNs отсутствуют.

Android boot receiver есть, но OEM-independent reboot guarantee остается OPEN.

### Device Media Ownership

Selected camera/gallery file — temporary input.

Copied Aveli visit photo становится local professional workspace data.

### Connectivity Not Promoted

`connectivity_plus` — thin OS network hint и остается contextual в frontend access/offline behavior.

Это не отдельный external provider.

## Verified Absent Integrations

Current evidence не показывает:

```text
Firebase
Sentry
third-party analytics SDK
FCM push
crash-reporting SDK
```

`AccessAnalytics` — local `debugPrint`, не external analytics provider.

## Open Gaps

| Gap | Evidence Needed |
|---|---|
| Production App Store product ids | App Store Connect / RevenueCat dashboard evidence. |
| Play base plans/offers | Play Console / RevenueCat linking evidence. |
| Subscription group/offering layout | Provider dashboards. |
| Exact Android minSdk integer | Build output / Flutter SDK resolved value. |
| iOS StoreKit project config | Xcode/App Store configuration evidence. |
| Guaranteed reboot reminder behavior | Device/OEM QA. |
| Android backup behavior | Explicit backup rules/native audit. |
| RevenueCat anonymous identity merge | SDK traces/dashboard behavior. |
| Real-store E2E purchase coverage | Store sandbox/device evidence. |
| Certificate pinning | Security implementation/review. |

## Baseline Recommendation

После merge провести repository-level audit:

- relative links;
- RU/EN parity;
- duplicate old `07-Integrations/` knowledge;
- integration ownership consistency;
- frontend/backend/database references;
- stale test-fixture product ids.

Если audit проходит:

```text
Baseline: Stable
```
