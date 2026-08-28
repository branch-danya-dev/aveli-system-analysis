# System Review — Closure Register

> Final classification прежних open questions.

## Статус

Для current Aveli baseline **нет unresolved architecture/documentation blockers**.

## Resolved

- Client delete: archive сохраняет history; permanent delete only without appointment references.
- Service lifecycle: separate deactivate state отсутствует; historical appointment meaning нельзя invalidated.
- Appointment conflict: business contract остаётся на verified level — configured scheduling rules + conflict rejection; exact interval algorithm не invented.
- Payment: one aggregate record per appointment; partial/full в нём же.
- Offline duration: policy-based; server deadline preferred, 72h client default.
- Repeated profile DELETE: one authenticated delete guaranteed; repeated post-delete HTTP не guaranteed.
- Verification/reset routes: 501 future stubs.
- Multi-account UI: out of current scope.
- Legacy DB claim: helper exists, shipped UI path отсутствует.
- Drift owner: `frontend/stack/drift/`.
- Prisma owner: `backend/stack/prisma/`.
- `operations/`: отдельная perspective current Aveli не нужна.
- SSAD: required perspectives, не fixed folder template.

## Accepted Limitations

Service duration naming discrepancy, unknown dedicated 429 body, Android backup policy, OEM-independent reboot reminder guarantee, certificate pinning/root detection decisions are explicit limitations or threat-model decisions, не hidden blockers.

## External / Release Evidence

Store product/group/base-plan ids, RevenueCat dashboard linking, StoreKit/provider config, build evidence, real-store E2E, per-release tests, signing/provisioning/secrets verify per production environment.

## Future Product Decisions

Export/import merge semantics, numeric performance targets, RevenueCat anonymous transfer behavior require new product decision/evidence and are not current architecture truth.

## Architecture-Change Triggers

Multi-device sync, cloud backup, public booking backend, shared/team workspace, server messaging, organization/employee management → new analysis branch.
