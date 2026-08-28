# Business

> Canonical business-level documentation Aveli: product context, scope, requirements, processes, acceptance и traceability.

## Статус

**Baseline: Stable**

Business baseline согласована с current product boundary и implementation-backed system model. Future product choices явно классифицированы в [`../system/review/open-questions.ru.md`](../system/review/open-questions.ru.md).

## Назначение

`business/` объясняет **что такое Aveli, зачем продукт существует, какое behavior система должна предоставлять и какие product rules/boundaries должны сохраняться**.

Business documentation владеет intent и observable behavior. Technical implementation canonical в:

```text
database/
backend/
frontend/
integrations/
system/
```

## Responsibility

Area владеет product context/goals, scope, business rules, FR/NFR, acceptance criteria, processes и traceability от business intent к technical ownership/verification.

## Boundary

Business documentation отвечает:

```text
Почему product существует?
Кто его использует?
Что in / out of scope?
Какое behavior требуется?
Какие rules должны оставаться true?
Какой результат acceptable?
```

Она не владеет конкретными DB schemas, framework choices, API payload internals или deployment configuration, если technical constraint сам не меняет product behavior или external contract.

## Структура

| Area | Responsibility |
|---|---|
| `context/` | Product background, users, goals, positioning. |
| `scope/` | Product/system boundary и exclusions. |
| `requirements/` | Rules, FR, NFR, acceptance criteria. |
| `processes/` | User/product workflows. |
| `traceability/` | Business → verification → technical ownership. |
| `diagrams/` | Business knowledge maps. |

Business map: [`diagrams/business-map.puml`](diagrams/business-map.puml)

## Путь чтения

```text
context/
→ scope/
→ requirements/
→ processes/
→ traceability/
```

Implementation: [`../database/`](../database/), [`../backend/`](../backend/), [`../frontend/`](../frontend/), [`../integrations/`](../integrations/), [`../system/`](../system/).

## Documentation Rules

[`../rules.ru.md`](../rules.ru.md)
