# Frontend Security Controls

## Verified Controls

| Concern | Current behavior |
|---|---|
| Release API transport gate | Release build rejects non-HTTPS API base. |
| Release host safety | Loopback/emulator API hosts are rejected in release. |
| Standalone bypass | `AVELI_STANDALONE` is rejected by release gate. |
| Session credentials | Stored in `flutter_secure_storage`. |
| Access snapshot | Stored in secure storage per user. |
| Local workspace isolation | Per-user `aveli_<userId>.sqlite`. |
| Visit-photo isolation | Per-user file root. |
| RevenueCat keys | Only public mobile SDK keys live in client config. |
| Backend secrets | Not part of client configuration. |
| Token logging | No deliberate token `debugPrint` in verified auth path. |
| Logout | Cancels reminders, clears secure access/session state, closes DB. |
| Profile delete | Removes local DB/photos/snapshot/session data. |

## Release Configuration

Current client dart-defines include:

```text
AVELI_API_BASE
AVELI_STANDALONE
AVELI_DEBUG_SEED
REVENUECAT_IOS_API_KEY
REVENUECAT_ANDROID_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Default development API base may use:

```text
http://10.0.2.2:3000
```

but release gate requires ship-safe HTTPS configuration.

## Trust Boundary

```text
Client UI state
     ≠
server authorization
```

Access unlock depends on server-produced `AccessState` or a previously verified secure snapshot within policy.

RevenueCat mobile `CustomerInfo` is not final entitlement authority.

## Open Security Review Items

Not established by the current evidence:

- certificate pinning;
- root/jailbreak detection;
- Android backup behavior for SQLite/files.

These should remain open rather than assumed.
