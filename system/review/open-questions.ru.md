# System-Wide Open Questions

> Explicit questions, оставшиеся unresolved после current system synthesis.

Не все являются blockers.

Это backlog для final polish.

## Business / Domain

- Exact final client archive/delete rules.
- Exact service deactivate/delete lifecycle.
- Complete appointment-conflict rule set.
- Complete payment lifecycle и expected future multi-payment support.
- Import/export merge/conflict semantics.
- Remaining measurable performance thresholds.

## Access / Account

- Repeated HTTP `DELETE /v1/auth/me` после account=`deleted`: service-level idempotency vs JWT active-user guard.
- Exact throttling error contract, если он нужен как public API contract.
- Остаются ли unimplemented email verification/password reset routes future scope или их убрать из product-facing docs.

## Local Workspace / Platform

- Android backup behavior SQLite/visit-photo files.
- Guaranteed notification restoration после arbitrary OEM reboot без reopening Aveli.
- Final legacy `aveli.db` migration/claim policy.
- Multi-account UI intentionally long-term out of scope или просто not implemented.

## Store / Billing

- Production App Store product ids.
- Google Play product/base-plan ids.
- Subscription group/offering configuration.
- RevenueCat anonymous identity merge/transfer behavior.
- Real-store end-to-end purchase evidence.

## Security

- Certificate pinning decision.
- Root/jailbreak detection decision.
- Возможно ли считать эти controls unnecessary для current risk model.

## Documentation / Methodology

- Final physical ownership Drift docs после удаления database duplicate.
- Final physical ownership Prisma docs после удаления database duplicate.
- Нужен ли `operations/` как отдельный perspective в current Aveli scope.
- Останутся ли perspectives mandatory directories или evolve в mandatory analytical perspectives с system-dependent physical structure.

## Rule

OPEN question должен превратиться в:

```text
resolved decision
accepted current limitation
explicit future task
out of scope
new architecture branch
```

Он не должен оставаться silently ambiguous.
