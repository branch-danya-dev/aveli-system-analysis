# Offline Policy

## Local Workspace

Normal workspace operations continue against local persistence without backend synchronization.

## Access Verification

Verified startup cases:

| Scenario | Result |
|---|---|
| Online | Fetch `/v1/access`, persist snapshot. |
| Offline + valid snapshot | Use cached access if verification policy allows. |
| Offline + no snapshot | Access needs network. |
| Cached verification deadline expired | `needsNetwork`. |
| Denied/expired cached state | Snapshot may be cleared and gate shown. |

## Connectivity Hint

`networkAvailableProvider` uses `connectivity_plus`.

If the connectivity plugin itself errors, the provider currently **fails open** and yields `true`.

HTTP/API failures are still handled separately and may cause stale-cache fallback.

## Network Restoration

Recovery is user/lifecycle driven:

- retry from access gate;
- app resume triggers access/billing refresh behavior.

## Failure Isolation

Backend/network failure must not delete the professional workspace.

## No Cloud Sync

Network restoration does not upload normal clients, appointments, payments, notes or photos.
