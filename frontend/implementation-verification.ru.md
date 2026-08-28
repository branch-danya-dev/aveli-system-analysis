# Frontend Implementation Verification

> Record сверки prior architecture documentation с current Flutter client implementation.

## Статус

**Applied to this frontend implementation pass**

Файл является audit record. Canonical knowledge должно находиться в owning frontend area.

## Verified Baseline

Current client:

```text
Aveli 0.2.2+4
Dart SDK ^3.7.0
feature-first Flutter architecture
Riverpod 2.6.1
go_router 14.8.1
Drift 2.34.3
```

Verified startup/access/workspace model отражена в frontend documentation.

## Important Corrections Applied

### Legacy Database Claim

Prior docs могут подразумевать automatic legacy `aveli.db` claim после first login.

Current source:

```text
LocalDatabaseManager supports claimLegacyIfPresent
BUT
no current UI/client caller enables it
```

Legacy auto-claim не документируется как shipped behavior.

### Access Authority

RevenueCat `CustomerInfo` не unlocks workspace напрямую.

```text
purchase/restore
→ RevenueCat result
→ POST /v1/billing/sync
→ backend AccessStatusView
→ AccessState
→ Access Gate
```

### Offline Grace

Client содержит 72-hour default в `AccessVerificationPolicy`.

При наличии backend `nextVerificationRequiredAt` preferred.

72h — current client default, не universal immutable product constant.

### Logout vs Profile Delete

Logout:

```text
preserve SQLite + photos
```

Profile delete:

```text
delete local SQLite + user photos + snapshot + session state
```

Lifecycles разделены явно.

### Logout All

Backend поддерживает logout-all, но current Flutter client/UI method не найден.

### Email Verification / Password Reset

Client routes/UI могут существовать, но backend возвращает `501 AUTH_NOT_IMPLEMENTED`.

Это не shipped end-to-end capability.

## Verified Source Mismatches / Gaps

| Topic | Current source reality |
|---|---|
| Legacy DB claim | Helper есть, UI caller отсутствует. |
| Logout all devices | Backend only, Flutter method/UI нет. |
| Profile phone/email verify | Local flags; real SMS/email verification API нет. |
| Public listing/lifecycle profile fields | Local `app_settings`, no server sync. |
| Brand string `Avile` | Есть в части generated/localized source; product — Aveli. |
| RC direct unlock | Не используется; backend sync required. |

## Open Questions

- Android backup behavior SQLite/files;
- certificate pinning;
- root/jailbreak detection;
- exact production store product ids;
- future multi-account product behavior;
- reboot-specific reminder restoration independent of app resume;
- current full test-suite pass rate;
- real-store E2E purchase coverage.

## Baseline Recommendation

После merge провести repository-level frontend audit по аналогии с `database/` и `backend/`.

Если links, RU/EN maintenance и ownership boundaries проходят audit, frontend можно перевести в:

```text
Baseline: Stable
```
