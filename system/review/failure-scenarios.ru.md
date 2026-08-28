# Aveli — Cross-System Failure Scenarios

> Consolidated non-happy-path scenarios, сохраненные из legacy offline/error documentation перед удалением numbered architecture.

## Назначение

Документ сохраняет **cross-system failure behavior**, пересекающий authentication, access, billing, local persistence, account switching, reminders и external services.

Он не является canonical owner component-specific implementation details или domain rules.

Detailed behavior остается в:

- [`../../frontend/`](../../frontend/)
- [`../../backend/`](../../backend/)
- [`../../database/`](../../database/)
- [`../../integrations/`](../../integrations/)
- [`../../business/requirements/`](../../business/requirements/)

Задача файла — сделать system-level **failure isolation и recovery** явными.

---

## Failure Principle

Для всех scenarios:

```text
Technical Failure
      ≠
Delete User Work
```

Authentication, access, billing, backend и external-service failures должны быть изолированы от locally owned professional workspace data whenever possible.

---

## FS-001 — Invalid Credentials

```text
Sign in
   ↓
Credentials invalid
   ↓
Authentication rejected
   ↓
No session created
```

Expected:

- unidentified user не получает authenticated session;
- local workspace не открывается от имени unidentified identity;
- existing local workspace files не удаляются.

Canonical owners:

- [`../../frontend/auth/`](../../frontend/auth/)
- [`../../backend/auth/`](../../backend/auth/)

---

## FS-002 — Access Token Expired

```text
Authenticated request
      ↓
Access token expired
      ↓
Refresh session
     /       \
 success     failure
   ↓            ↓
continue    re-authenticate
```

Expected:

- successful refresh продолжает session;
- failed refresh требует authentication;
- local workspace data остаются unchanged.

---

## FS-003 — Missing or Corrupted Client Session State

Если secure client state incomplete или untrusted:

```text
Restore session
      ↓
Session state invalid
      ↓
Do not trust identity
      ↓
Return to authentication
```

Authentication-state problem не должен решаться удалением local database.

---

## FS-004 — Reinstall During Active Trial

```text
Trial active
   ↓
Application reinstalled
   ↓
Sign in to same account
   ↓
Backend returns account-owned trial state
```

Expected:

- reinstall не создает новый trial;
- local application state не authoritative для trial creation.

---

## FS-005 — Local Workspace Deleted During Trial

```text
Local workspace deleted
        ≠
Backend trial reset
```

Удаление local professional data не reset account-owned trial period.

Canonical:

[`../../backend/access/`](../../backend/access/)

---

## FS-006 — Trial or Access Changes While App Is Open

Если effective access state меняется, пока user уже в workspace, приложение должно re-evaluate access согласно current verification policy.

```text
Access state changes
      ↓
Verification occurs when required
      ↓
Access Gate may change
```

Но:

```text
Access change
      ≠
Workspace reset
```

---

## FS-007 — Multiple Access Sources Are Active

Пример:

```text
Trial        = active
Subscription = active
Lifetime     = active
```

Backend resolves один effective access result согласно canonical precedence.

Frontend использует resolved result, а не самостоятельно combines access sources.

Canonical:

[`../../backend/access/access-resolution.ru.md`](../../backend/access/access-resolution.ru.md)

---

## FS-008 — One Access Source Expires but Another Remains Valid

Пример:

```text
Subscription = expired
Manual Grant = valid
```

Expected:

- workspace access остается available;
- effective source меняется согласно backend resolution;
- local workspace не мутируется из-за expiry одного access source.

---

## FS-009 — No Access but Local Data Exists

```text
No valid access
      ↓
Access Gate
```

Existing:

```text
SQLite workspace
visit photos
professional history
```

сохраняются.

Это access-state condition, не data-deletion event.

---

## FS-010 — App Starts Offline With Valid Trusted Snapshot

```text
No network
   ↓
Trusted snapshot available
   ↓
Verification policy accepts it
   ↓
Workspace opens
```

User может продолжать local work, пока cached access trusted current policy.

Canonical:

[`../../frontend/offline/`](../../frontend/offline/)

---

## FS-011 — App Starts Offline Without Trusted Snapshot

```text
No network
   ↓
No trusted snapshot
   ↓
Access cannot be verified
   ↓
Network verification required
```

System не должен угадывать, что access valid.

Local professional data сохраняются.

---

## FS-012 — Offline Verification Window Expires

```text
Cached access previously valid
        ↓
Verification deadline exhausted
        ↓
Backend verification required
```

Temporary cached trust не должен превращаться в permanent offline authorization.

Expected:

- workspace authorization может стать unavailable;
- local workspace data остаются stored.

---

## FS-013 — Purchase Succeeds but Backend Reconciliation Fails

```text
Store purchase succeeds
        ↓
RevenueCat state updated
        ↓
POST /v1/billing/sync fails
```

Expected:

- purchase не считается исчезнувшим;
- local workspace data unchanged;
- billing reconciliation можно retry;
- Aveli workspace access refreshes только после successful backend reconciliation или другого valid access source.

Boundary:

```text
Provider purchase result
        ≠
Aveli workspace access
```

Canonical:

- [`../../frontend/billing/`](../../frontend/billing/)
- [`../../backend/billing/`](../../backend/billing/)
- [`../../integrations/revenuecat/`](../../integrations/revenuecat/)

---

## FS-014 — Backend Reconciliation Succeeds but Client State Is Stale

Backend уже может иметь valid normalized subscription, пока frontend показывает stale access.

Recovery:

```text
Backend state valid
      ↓
Client refreshes AccessState
      ↓
Current access result restored
```

Recovery не требует recreate account/workspace data.

---

## FS-015 — Restore Finds No Active Subscription

```text
Restore purchase
      ↓
No active support entitlement
      ↓
No subscription access
```

Это valid business outcome, не обязательно technical failure.

Другие access sources могут отдельно давать access.

---

## FS-016 — RevenueCat Webhook Is Late or Repeated

Mobile billing sync и webhook delivery могут происходить в разное время.

Expected:

```text
Webhook received
      ↓
Idempotency check
      ↓
Provider reconciliation
      ↓
Normalized subscription state
```

Repeated/delayed webhook не должен создавать contradictory access state.

Canonical:

[`../../integrations/revenuecat/webhooks.ru.md`](../../integrations/revenuecat/webhooks.ru.md)

---

## FS-017 — Logout With Existing Professional Data

Logout завершает active identity/session context без удаления workspace.

```text
Logout
  ↓
Clear active client session/access state
  ↓
Close current user database
  ↓
Cancel account-specific reminders
```

Preserved:

```text
local database
visit photos
professional history
```

Canonical lifecycle:

[`../flows/logout-and-profile-delete.ru.md`](../flows/logout-and-profile-delete.ru.md)

---

## FS-018 — Different User Signs In

```text
User A logout
      ↓
User B login
      ↓
Open User B local workspace
```

Expected:

- User A records не появляются у User B;
- database/media namespaces isolated authenticated identity.

---

## FS-019 — Local Database Migration Fails

Local migration failure — data-integrity problem.

System не должен silently recreate empty DB, если это уничтожит recoverable workspace data.

```text
Migration failure
      ↓
Surface / isolate persistence failure
      ↓
Preserve recoverable data
```

Canonical:

[`../../database/local/migrations/`](../../database/local/migrations/)

---

## FS-020 — Visit Photo Metadata Exists but File Is Missing

```text
Photo metadata exists
        +
Physical file missing
        ↓
Media item unavailable
```

Expected:

- missing media item handles as unavailable;
- whole visit/appointment не считается corrupted только из-за одного missing file.

---

## FS-021 — Reminder Refers to a Missing Appointment

Notification payload может ссылаться на уже отсутствующий appointment.

```text
Notification tap
      ↓
Appointment not found
      ↓
Safe navigation fallback
```

Navigation не должна catastrophic fail.

---

## FS-022 — Logout Before Reminder Fires

Account-specific reminders отменяются на logout.

Это предотвращает appearance appointment information прошлого account после sign-in другого user.

---

## FS-023 — External Exchange-Rate Service Is Unavailable

```text
Rate refresh fails
      ↓
Currency conversion unavailable / fallback
```

Expected:

- unrelated workspace operations продолжаются;
- existing local professional data unchanged.

Canonical:

[`../../integrations/exchange-rate/`](../../integrations/exchange-rate/)

---

## FS-024 — Store Service Is Unavailable

Если Apple App Store / Google Play не могут выполнить purchase/restore:

- current access state не меняется только из-за store outage;
- user может остаться на Access Gate, если другого access source нет;
- local workspace data unaffected.

---

## Domain-Specific Failures

Legacy document также содержал appointment/client/service/payment edge cases.

Они намеренно **не становятся canonical здесь**.

Примеры:

```text
appointment conflict
appointment state transition
referenced client deletion
duplicate payment action
invalid payment state
```

Их final rules принадлежат business/domain requirements и owning frontend behavior.

При final polish unresolved domain cases нужно reconcile с:

[`../../business/requirements/`](../../business/requirements/)

---

## Summary

Main cross-system failure categories:

```text
authentication recovery
access/trial transitions
offline verification
billing reconciliation
account/workspace isolation
local persistence integrity
reminder recovery
external service failure
```

Common expectation:

```text
deterministic behavior
+
recoverable state
+
failure isolation
+
protection of locally owned work
```
