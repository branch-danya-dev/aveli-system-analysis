# System-Wide Open Questions

> Explicit questions that remain unresolved after current system synthesis.

These are not all blockers.

They are the backlog to evaluate during final polish.

## Business / Domain

- Exact final client archive/delete rules.
- Exact service deactivate/delete lifecycle.
- Complete appointment-conflict rule set.
- Complete payment lifecycle and whether future multi-payment support is expected.
- Import/export merge/conflict semantics.
- Remaining measurable performance thresholds.

## Access / Account

- Repeated HTTP `DELETE /v1/auth/me` behavior after the account becomes `deleted`: service-level idempotency vs JWT active-user guard.
- Exact throttling error contract if it needs to become part of public API documentation.
- Whether currently unimplemented email verification/password reset routes remain future scope or should be removed from product-facing docs.

## Local Workspace / Platform

- Android backup behavior for SQLite and visit-photo files.
- Guaranteed notification restoration after arbitrary device/OEM reboot without reopening Aveli.
- Final legacy `aveli.db` migration/claim policy.
- Whether multi-account UI is intentionally out of scope long-term or simply not implemented.

## Store / Billing

- Production App Store product ids.
- Google Play product/base-plan ids.
- Subscription group / offering configuration.
- RevenueCat anonymous identity merge/transfer behavior.
- Real-store end-to-end purchase evidence.

## Security

- Certificate pinning decision.
- Root/jailbreak detection decision.
- Whether these controls are intentionally unnecessary for current risk model.

## Documentation / Methodology

- Final physical ownership of Drift docs after removing database duplicate.
- Final physical ownership of Prisma docs after removing database duplicate.
- Whether `operations/` is needed as a real perspective for current Aveli scope.
- Whether all perspectives remain mandatory directories or evolve into mandatory analytical perspectives with system-dependent physical structure.

## Rule

An OPEN question should become one of:

```text
resolved decision
accepted current limitation
explicit future task
out of scope
new architecture branch
```

It should not remain silently ambiguous.
