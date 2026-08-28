# Offline Policy

## Local Workspace

Normal workspace operations продолжают работать с local persistence без backend synchronization.

## Access Verification

Verified startup cases:

| Scenario | Result |
|---|---|
| Online | Fetch `/v1/access`, persist snapshot. |
| Offline + valid snapshot | Use cached access если policy разрешает. |
| Offline + no snapshot | Access needs network. |
| Cached verification deadline expired | `needsNetwork`. |
| Denied/expired cached state | Snapshot может быть cleared, показывается gate. |

## Connectivity Hint

`networkAvailableProvider` использует `connectivity_plus`.

Если сам connectivity plugin падает, provider сейчас **fails open** и возвращает `true`.

HTTP/API failures обрабатываются отдельно и могут привести к stale-cache fallback.

## Network Restoration

Recovery запускается user/lifecycle events:

- retry из access gate;
- app resume triggers access/billing refresh.

## Failure Isolation

Backend/network failure не должен удалять professional workspace.

## No Cloud Sync

Network restoration не загружает на backend clients, appointments, payments, notes или photos.
