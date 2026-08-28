# Frontend Testing

> Verified automated-test coverage areas of the Flutter client.

## Current Evidence

Approximately 135 test files are present under `test/`.

## Coverage Areas

| Area | Verified examples |
|---|---|
| Database / repositories | CRUD, constraints, migrations, payments, visit flow. |
| Auth lifecycle | Logout snapshot/reminder/session behavior. |
| Access | Gate decision, snapshot persistence, repository behavior. |
| Router | Paths and date-query routing. |
| Bootstrap | Bootstrap destinations and QA matrix. |
| Purchase / RevenueCat | Purchase service and flow-result behavior. |
| Reminders | Launch details and reminder service. |
| Release gate | Ship-safe API configuration. |
| Widgets | Screen/shell/calendar/today smoke behavior. |

In-memory Drift tests use:

```text
NativeDatabase.memory()
```

## Open Verification

The evidence document does not establish:

- fresh overall `flutter test` pass rate;
- real-store end-to-end purchase tests.

Those should not be reported as verified green.
