# Frontend Security Controls

## Verified Controls

| Concern | Current behavior |
|---|---|
| Release API transport gate | Release build rejects non-HTTPS API base. |
| Release host safety | Loopback/emulator API hosts rejected in release. |
| Standalone bypass | `AVELI_STANDALONE` rejected release gate. |
| Session credentials | Stored in `flutter_secure_storage`. |
| Access snapshot | Stored secure storage per user. |
| Local workspace isolation | Per-user `aveli_<userId>.sqlite`. |
| Visit-photo isolation | Per-user file root. |
| RevenueCat keys | Только public mobile SDK keys в client config. |
| Backend secrets | Не входят в client configuration. |
| Token logging | Нет deliberate token `debugPrint` в verified auth path. |
| Logout | Cancels reminders, clears secure access/session state, closes DB. |
| Profile delete | Removes local DB/photos/snapshot/session data. |

## Release Configuration

Current client dart-defines:

```text
AVELI_API_BASE
AVELI_STANDALONE
AVELI_DEBUG_SEED
REVENUECAT_IOS_API_KEY
REVENUECAT_ANDROID_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Default development API base может быть:

```text
http://10.0.2.2:3000
```

но release gate требует ship-safe HTTPS configuration.

## Trust Boundary

```text
Client UI state
     ≠
server authorization
```

Workspace unlock зависит от server-produced `AccessState` или previously verified secure snapshot в пределах policy.

RevenueCat mobile `CustomerInfo` не final entitlement authority.

## Open Security Review Items

Current evidence не устанавливает:

- certificate pinning;
- root/jailbreak detection;
- Android backup behavior для SQLite/files.

Эти пункты остаются open.
