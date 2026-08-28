# Frontend Testing

> Verified automated-test coverage Flutter client.

## Current Evidence

Под `test/` находится примерно 135 test files.

## Coverage Areas

| Area | Verified examples |
|---|---|
| Database / repositories | CRUD, constraints, migrations, payments, visit flow. |
| Auth lifecycle | Logout snapshot/reminder/session behavior. |
| Access | Gate decision, snapshot persistence, repository behavior. |
| Router | Paths и date-query routing. |
| Bootstrap | Bootstrap destinations и QA matrix. |
| Purchase / RevenueCat | Purchase service и flow-result behavior. |
| Reminders | Launch details и reminder service. |
| Release gate | Ship-safe API configuration. |
| Widgets | Screen/shell/calendar/today smoke behavior. |

In-memory Drift tests:

```text
NativeDatabase.memory()
```

## Open Verification

Evidence document не устанавливает:

- fresh overall `flutter test` pass rate;
- real-store end-to-end purchase tests.

Их нельзя описывать как verified green.
