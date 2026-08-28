# Aveli — Traceability Matrix

<p align="center"><a href="traceability-matrix.md"><b>English</b></a> · <a href="traceability-matrix.ru.md">Русский</a></p>

> Connects business rules, requirements, acceptance, quality expectations, and real technical ownership.

## Status

**Baseline: Stable**

| Area | Business Rules | FR | NFR | Acceptance | Technical Ownership | Status |
|---|---|---|---|---|---|---|
| Authentication | BR-043–047 | FR-001–005 | NFR-011, 013–015, 049 | AC-001–005, AC-056 | `backend/auth/`, `frontend/auth/`, `frontend/bootstrap/` | Covered |
| Access decision | BR-001–007 | FR-008–011 | NFR-019–021 | AC-009–011 | `backend/access/`, `frontend/access/` | Covered |
| Trial lifecycle | BR-008–013 | FR-006–007 | NFR-022 | AC-006–008 | `backend/access/`, `database/server/` | Covered |
| Access expiry/restoration | BR-025–026 | FR-012–013, 061–062 | NFR-010, 028–029 | AC-012–013, 025, 050 | `backend/access/`, `frontend/access/`, `frontend/storage/`, `database/` | Covered |
| Subscription | BR-014–019 | FR-014–019 | NFR-021, 030, 038–039 | AC-014–020 | `frontend/billing/`, `backend/billing/`, `integrations/revenuecat/` | Covered |
| Clients | BR-039–042, 062–063 | FR-020–026 | NFR-023, 027, 035 | AC-026–030 | `frontend/workspace/feature-map.md`, `database/local/entities/clients.md`, `integrations/device-contacts/` | Covered |
| Services | BR-064–065 | FR-027–029 | NFR-023 | AC-057–058 | `frontend/workspace/feature-map.md`, `database/local/entities/services.md` | Covered |
| Appointments / visits | BR-027–033, 066–068 | FR-030–038 | NFR-023, 025, 034 | AC-031–038 | `frontend/workspace/feature-map.md`, `database/local/entities/appointments.md`, `frontend/errors/` | Covered |
| Payments | BR-034–038, 069–071 | FR-039–042 | NFR-023, 026 | AC-039–042, AC-064 | `frontend/workspace/feature-map.md`, `database/local/entities/payments.md` | Covered |
| Today / Calendar | BR-029–031, 066–068 | FR-043–046 | NFR-033–034 | AC-032, 059–060 | `frontend/workspace/feature-map.md`, `frontend/navigation/` | Covered |
| Reminders | BR-053–056 | FR-047–050 | NFR-032 | AC-043–046 | `frontend/notifications/`, `integrations/device-notifications/` | Covered |
| Profile / settings / transfer | BR-072 | FR-051–057 | NFR-040–042 | AC-061–063 | `frontend/workspace/feature-map.md`, `database/local/entities/app_settings.md`, `integrations/device-handoff/` | Covered at current contract |
| Workspace isolation | BR-020–026, 047 | FR-058–062 | NFR-006–010, 027 | AC-021–025 | `database/`, `frontend/storage/`, `backend/auth/` | Covered |
| Offline workspace | BR-048–052 | FR-063–066 | NFR-001–005, 028–029, 036 | AC-047–050 | `frontend/offline/`, `frontend/access/`, `backend/access/` | Covered |


## Non-Functional Verification Ownership

| Quality Area | Technical Verification |
|---|---|
| Offline / access consistency | `frontend/access/`, `frontend/offline/`, `backend/access/`, `system/review/failure-scenarios.md` |
| Privacy / isolation | `database/`, `frontend/storage/`, `system/trust/` |
| Security | `frontend/security/`, `backend/security/`, `integrations/`, `system/trust/` |
| Data integrity | `database/`, `frontend/testing/`, owning workspace behavior |
| Reliability / recovery | `system/review/failure-scenarios.md`, component tests |
| Performance | future calibrated measurements; see closure register |
| Release readiness | `system/review/release-readiness.md` |
| Testability | `frontend/testing/` plus backend/integration implementation evidence |

There is no standalone `operations/` owner in the current Aveli baseline.

## Coverage Result

All current functional product areas have a business/requirement → acceptance → technical-owner path at the current contract level.

Remaining work is explicitly classified as future product decision, external/provider evidence, platform QA evidence, future measurable calibration, or architecture-change trigger.

See [`../../system/review/open-questions.md`](../../system/review/open-questions.md).
