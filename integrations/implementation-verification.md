# Integrations Implementation Verification

> Reconciliation record between previous integration documentation and current implementation evidence.

## Status

**Applied to this integration implementation pass**

Canonical knowledge should live in the owning integration directory.

## Verified Baseline

Evidence covers:

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

The NestJS account API is an internal Aveli boundary.

It remains canonical in:

```text
backend/api/
frontend/
```

and is not promoted as a provider integration.

### RevenueCat Client Does Not Unlock Workspace

Verified flow:

```text
RevenueCat purchase/restore
→ backend billing sync
→ AccessStatusView
→ Access Gate
```

### Store Product IDs Remain OPEN

Names visible only in backend test fixtures:

```text
aveli_support_monthly
aveli_support_yearly
```

are not documented as verified production App Store / Play product ids.

### RevenueCat Gateway Has No Explicit Retry/Timeout

Verified current backend gateway:

```text
retry: none
explicit fetch timeout: none
```

This is now visible rather than inferred away.

### Webhook Event Type Is Not Access Logic

Webhook events trigger RevenueCat REST reconciliation.

The backend does not encode workspace access directly from `event_type`.

### Contacts Are Read-Only

Device contact integration reads and imports into Aveli local records.

No write-back path is present.

### Notifications Are Local

FCM/APNs are absent.

The Android boot receiver exists, but OEM-independent reboot guarantee remains OPEN.

### Device Media Ownership

The selected camera/gallery file is temporary input.

Aveli's copied visit photo becomes local professional workspace data.

### Connectivity Not Promoted

`connectivity_plus` is a thin OS network hint and remains contextual to frontend access/offline behavior.

It is not treated as a separate external provider.

## Verified Absent Integrations

Current dependency/source evidence shows no:

```text
Firebase
Sentry
third-party analytics SDK
FCM push
crash-reporting SDK
```

`AccessAnalytics` is local `debugPrint`, not an external analytics provider.

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

After merge, run a repository-level audit for:

- relative links;
- RU/EN parity;
- duplicate old `07-Integrations/` knowledge;
- integration ownership consistency;
- references to frontend/backend/database;
- stale test-fixture product ids.

If that passes, promote the integration branch to:

```text
Baseline: Stable
```
