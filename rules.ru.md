# Aveli System Analysis Repository Rules

> **Методология:** System-Structured Analysis Documentation (SSAD)  
> **Статус:** Repository governance baseline  
> **Scope:** Нормативные правила структуры, написания, связывания и review документации в этом репозитории.

[English version](rules.md) · [Методология](methodology.ru.md)

---

## 1. Primary Rule

> **Документация отражает систему.**

Repository structure ДОЛЖНА прежде всего следовать реальным responsibilities, boundaries и ownership, а не категориям аналитических artifacts.

Не создавай directories только потому, что generic methodology example их содержит. Area появляется, когда системе действительно нужен owner соответствующего knowledge.

---

## 2. Required Analytical Perspectives

Каждая анализируемая система ДОЛЖНА явно отвечать:

```text
Какой product/problem существует?
Что входит и не входит в scope?
Какое behavior требуется?
Какие data существуют и кто ими владеет?
Какие runtime components владеют responsibilities?
Как components взаимодействуют?
Что пересекает external system boundary?
Какие technologies поддерживают responsibilities?
Какие trust/failure/security constraints существуют?
Как принимается или проверяется важное behavior?
Как components образуют одну систему?
```

Это обязательные **perspectives**, а не обязательные названия папок.

Current Aveli:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

`operations/`, `worker/`, `plugin/`, `gateway/` и другие top-level areas добавляются только когда их оправдывает реальная архитектура.

---

## 3. Significant Directories Are Entry Points

Каждая meaningful human-readable documentation directory ДОЛЖНА по возможности иметь:

```text
README.md
README.ru.md
```

README ДОЛЖЕН объяснять purpose, responsibility, boundary, important contents и куда идти дальше.

README — navigation/context, а не duplicate всех child documents.

---

## 4. Language Rules

Human-readable project documentation поддерживается equivalent EN/RU pairs там, где area участвует в bilingual repository:

```text
document.md
document.ru.md
```

Directory entry points:

```text
README.md
README.ru.md
```

Обе версии ДОЛЖНЫ описывать одно behavior и decisions.

Machine-readable/language-neutral artifacts обычно существуют один раз:

```text
OpenAPI
SQL / DDL
JSON / YAML schemas
PlantUML / Mermaid with neutral labels
configuration examples
source identifiers
```

API paths, class names, database fields, configuration keys и другие implementation identifiers НЕ переводятся.

---

## 5. Business Boundary

`business/` владеет product intent:

```text
context
scope
requirements
business rules
processes
acceptance
traceability
```

Business docs ДОЛЖНЫ предпочитать product meaning implementation detail.

Предпочтительно:

```text
Account state is server-managed.
Professional workspace data is not synchronized between devices.
Access is controlled at workspace level.
```

Не добавляй technology names, если они не меняют materially product behavior, scope, external contract, regulation или user-visible capability.

Technical consequences должны ссылаться на owning technical area.

---

## 6. Canonical Ownership

У каждого важного concept должен быть один canonical owner.

До детализации определи:

```text
Кто решает?
Кто хранит canonical state?
Кто может менять?
Кто потребляет?
Кто проверяет?
```

Другие docs МОГУТ повторять небольшой context для readability, но должны ссылаться на canonical source, если факт существенный.

> **Do not duplicate knowledge. Duplicate context when necessary.**

Если два документа могут независимо прийти к разным определениям одного факта, ownership недостаточно ясен.

---

## 7. Data Documentation

`database/` владеет system data architecture и physical persistence knowledge.

Data modeling ДОЛЖЕН идти:

```text
ownership
→ conceptual model
→ logical model
→ physical model
→ constraints / migrations / lifecycle
```

System-wide conceptual/logical views НЕ должны стирать реальные storage boundaries.

Physical model принадлежит persistence owner.

Для Aveli:

```text
database/local/
→ SQLite workspace + files + persistence lifecycle

database/server/
→ PostgreSQL identity/access persistence
```

Frontend/backend описывают usage persistence, но НЕ переопределяют canonical schemas.

---

## 8. Runtime Component Documentation

Component area вроде `backend/` или `frontend/` владеет behavior этого runtime component.

Применимые concerns:

```text
responsibility boundary
architecture
state / behavior
interfaces
data usage
errors / failures
security
configuration
testing
technology stack
```

Создаются только applicable subdirectories. Не создавай empty capability folders заранее.

---

## 9. Technology Ownership and Stack

Technology является first-class analytical object, если materially влияет на architecture.

Canonical technology ownership ДОЛЖЕН следовать responsibility, которую technology primarily реализует.

Aveli target ownership:

```text
SQLite
PostgreSQL
→ database/stack/

Drift
Flutter
Riverpod
go_router
client HTTP
secure storage
mobile SDKs
→ frontend/stack/

NestJS
Prisma
REST style
JWT
Argon2id
→ backend/stack/

RevenueCat provider boundary
→ integrations/revenuecat/
```

Contextual usage МОЖЕТ описываться в других perspectives, но canonical technology knowledge НЕ дублируется.

Significant technology document SHOULD содержать, где известно:

- role;
- why used;
- where used;
- consumers/dependencies;
- limitations;
- criticality;
- replaceability;
- alternatives;
- links to contextual usage.

---

## 10. API and Internal Interfaces

Concrete contract принадлежит component, который owns interface.

Для Aveli:

```text
backend/api/
→ canonical backend HTTP contracts
```

`backend/stack/rest-api/` может описывать REST как style/technology, но НЕ дублирует endpoint contracts.

Frontend MUST reference backend API и описывать только client-specific consumption/retry/state/UI behavior.

Boundary:

```text
Frontend ↔ Backend
```

НЕ является external integration.

---

## 11. External Integrations

`integrations/` владеет boundaries, которые выходят за analyzed system.

Integration заслуживает canonical area, если имеет meaningful:

```text
external responsibility
contract / SDK
identity mapping
data crossing boundary
authentication / trust
configuration
failure semantics
retry / idempotency / reconciliation
platform constraints
```

Small OS/package usage может остаться contextual, если standalone integration owner не нужен.

Provider state НЕ должен автоматически становиться Aveli product authority, если Aveli владеет отдельным решением.

---

## 12. System Synthesis

`system/` владеет cross-component knowledge.

Он может содержать:

```text
system context
component model
cross-layer boundaries
end-to-end flows
data movement
trust / authority model
system invariants
boundary-changing evolution
cross-system failure scenarios
release readiness
open questions
```

Он НЕ должен полностью переописывать каждый component.

> **Слой может ссылаться на другой слой, но модель, описывающая сразу несколько слоёв, принадлежит их ближайшему общему системному уровню.**

Final system view SHOULD синтезироваться после достаточной стабилизации component views.

---

## 13. Tasks, Changes and Optional Areas

Task принадлежит области изменения. Он НЕ является inherently business artifact.

Не создавай `business/tasks/` или task dirs в каждом component по умолчанию.

Optional top-level areas вроде `operations/` также не создаются без реальных artifacts.

Если deployment, observability, backups, incident recovery или release operations становятся substantial, `operations/` МОЖЕТ появиться.

---

## 14. Traceability

High-impact behavior SHOULD быть traceable across abstraction levels.

```text
Business Need
    ↓
Business Rule / Requirement
    ↓
Component / Decision
    ↓
Interface / Data / Technology
    ↓
Acceptance
    ↓
Verification
```

Не каждая trace требует каждого шага.

Traceability должна позволять ответить:

```text
Почему это реализовано?
```

и:

```text
Как requirement сохраняет смысл в implementation?
```

Для Aveli canonical business traceability находится в `business/traceability/`.

---

## 15. Evidence and Truth Status

Documentation existing system ДОЛЖНА предпочитать current evidence архитектурному желанию.

Evidence:

```text
source code
schemas
API contracts
configuration
native project files
provider contracts
tests
runtime behavior
stakeholder decisions
```

При существенной uncertainty различай:

```text
VERIFIED
INFERRED
OPEN
```

Proposal ДОЛЖЕН быть помечен как target/proposed state.

Open questions остаются явными и не закрываются silently assumption.

---

## 16. Maturity

Docs МОГУТ использовать conceptual lifecycle:

```text
Draft
→ Baseline
→ Stable
```

`Stable` означает cross-check с relevant implementation, contract или stakeholder decision.

Detailed документ не становится Stable только из-за объёма.

---

## 17. Diagrams

Diagram поддерживает text, а не заменяет его.

Используй diagrams для system context, component relationships, sequences, state transitions, data models, integration boundaries и dependencies.

Machine-maintainable source SHOULD сохраняться.

Diagram НЕ должен противоречить canonical prose/contracts.

Rendered assets могут храниться отдельно.

---

## 18. Real Technical Artifacts

При наличии technical docs SHOULD reference реальные artifacts:

```text
OpenAPI
SQL / DDL
Prisma schema
Drift tables
JSON examples
configuration
migrations
code fragments
PlantUML
```

Example должен быть обозначен как:

```text
real implementation
simplified implementation
illustrative example
```

Illustrative code нельзя выдавать за verified source.

---

## 19. Failure, Security and Release Views

Failure/security/release knowledge принадлежит ближайшему полезному owner.

Component-local behavior остаётся у component. Cross-system behavior может синтезироваться в `system/`.

Для Aveli:

```text
frontend/security/
backend/security/
integrations/
system/trust/
system/review/failure-scenarios.*
system/review/release-readiness.*
```

Standalone `operations/` пока не требуется.

---

## 20. Legacy Migration and Refactoring

Перед удалением или restructuring старых docs:

1. определи новый canonical owner;
2. проверь old files на unique/orphan knowledge;
3. перенеси unique knowledge;
4. исправь cross-references;
5. удали duplicates;
6. повтори consistency review.

Нельзя удалять legacy branch только потому, что уже существует новая директория. Сначала нужно подтвердить отсутствие orphan knowledge.

---

## 21. AI / Automated Review

Automated review SHOULD проверять:

### Structure

- Tree отражает actual system?
- Important perspectives explicit?
- Optional dirs justified?

### Ownership

- У каждого important fact один canonical owner?
- Technologies принадлежат responsibility, которую primarily реализуют?
- Internal interfaces отделены от external integrations?

### Consistency

- EN/RU согласованы?
- Links resolve?
- Diagrams совпадают с prose?
- Examples совпадают с current contracts/schemas?
- Есть duplicate canonical knowledge?

### Traceability

- High-impact requirements ведут в implementation?
- Significant technologies связаны с concrete usage?
- OPEN questions visible?

### Readability

- Area понятна из README?
- Document объясняет meaning, а не просто перечисляет facts?

AI МОЖЕТ помогать review, но НЕ превращает unsupported inference в canonical truth.

---

## 22. Current Aveli Baseline

Aveli сейчас использует:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

Supporting content:

```text
screenshots/
renderer/
README.md
README.ru.md
methodology.md
methodology.ru.md
rules.md
rules.ru.md
```

Standalone `operations/` и `result/` analytical perspectives сейчас отсутствуют, потому что available knowledge не оправдывает отдельных top-level owners.

Если architecture изменится, structure тоже МОЖЕТ измениться.

---

## 23. Repository Quality Gate

Mature repository SHOULD позволять новому technical reader с минимальным поиском ответить:

```text
Что делает system?
Что in/out of scope?
Какие main components?
Какие data существуют?
Кто владеет data?
Как components communicate?
Какие external systems участвуют?
Какие technologies используются и почему?
Где они используются?
Кто authoritative для важных decisions?
Что происходит при failure dependencies?
Как verified важное behavior?
Какие вопросы остаются open?
Где canonical source каждого ответа?
```

Если ответы требуют чтения unrelated directories или противоречат друг другу, polish ещё не завершён.

---

## Core Summary

```text
Documentation mirrors the system.

Perspectives are required.
Folder templates are not.

Ownership comes before detail.

Business explains what and why.
Technical areas explain how.

Data progresses from ownership to persistence.

Technology ownership follows responsibility.
Usage is contextual.

Internal interfaces are not external integrations.

Canonical knowledge has one source of truth.
Related knowledge is connected by references.

Storage is hierarchical.
Knowledge is graph-based.

System is a synthesis layer.

Evidence beats architectural preference.

Open questions stay visible.

Human-readable documentation stays bilingual.
Machine-readable artifacts stay shared.
```
