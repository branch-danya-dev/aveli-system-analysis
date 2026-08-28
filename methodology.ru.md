# System-Structured Analysis Documentation

> **Рабочее название:** System-Structured Analysis Documentation (SSAD)  
> **Статус:** Развивающаяся методология, проверенная на кейсе Aveli  
> **Назначение:** Объяснить модель мышления, лежащую в основе структуры репозитория.

[English version](methodology.md) · [Правила репозитория](rules.ru.md)

---

## 1. Что такое SSAD

SSAD организует документацию системного анализа вокруг **реальных responsibilities и boundaries системы**, а не вокруг фиксированного набора типов аналитических артефактов.

Главная идея:

> **Документация должна отражать анализируемую систему.**

Поэтому репозиторий должен быть меньше похож на:

```text
requirements/
diagrams/
api/
security/
```

и больше — на саму систему:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

если эти области действительно существуют.

Физическое дерево — не сама методология. Методология — это ownership model, по которой это дерево строится.

Plugin, mobile product и distributed banking platform не должны искусственно иметь одинаковую структуру директорий.

---

## 2. Methodology и Rules

Этот файл объясняет, почему подход существует, как строится знание, как определяется ownership и как связаны system perspectives.

[`rules.ru.md`](rules.ru.md) задаёт нормативный contract репозитория: что должно быть документировано, что нельзя дублировать, где должно жить знание и как проверяется consistency.

Методология может развиваться из опыта реальных проектов. Rules должны меняться тогда, когда это развитие становится явным стандартом репозитория.

---

## 3. Репозиторий как модель системы

Документация должна поддерживать progressive depth:

```text
System
  ↓
Responsibility / Component
  ↓
Behavior / Contract / Data
  ↓
Technology
  ↓
Usage
  ↓
Implementation evidence
```

Разные читатели могут входить на разной глубине. Product reader может начать с `business/`; System Analyst перемещается между `business/`, `system/`, `database/`, `backend/`, `frontend/` и `integrations/`; developer может войти сразу через component, которым владеет.

Чтобы понять локальный concern, reader не должен читать репозиторий строго линейно.

---

## 4. Storage Is Hierarchical; Knowledge Is Graph-Based

Файлы и директории иерархичны, потому что ownership и navigation требуют дерева. Реальное system knowledge не иерархично.

Authentication может одновременно затрагивать:

```text
business requirements
backend session logic
server data
frontend bootstrap
secure storage
access policy
security constraints
```

Поэтому в репозитории существуют две структуры:

```text
Physical hierarchy
→ где живёт canonical knowledge

Cross-reference graph
→ как система реально связана
```

> **Storage is hierarchical. Knowledge is graph-based.**

---

## 5. Canonical Knowledge и Contextual Usage

У каждого важного факта должен быть один canonical owner.

Примеры:

```text
backend/api/
→ concrete backend HTTP contracts

database/
→ canonical data architecture и physical persistence

frontend/stack/drift/
→ Drift как frontend data-access technology

integrations/revenuecat/
→ cross-component RevenueCat boundary
```

Другие документы могут повторять столько context, сколько нужно для читаемости, но должны ссылаться на owner, а не создавать второй source of truth.

> **Do not duplicate knowledge. Duplicate context when necessary.**

---

## 6. Perspectives обязательны; folder template — нет

Проект должен отвечать на ключевые аналитические вопросы, даже если его physical tree отличается.

Как минимум analysis должен позволять понять:

```text
Product / business intent
System boundary and scope
Data ownership and lifecycle
Runtime responsibilities / components
Interfaces and external integrations
Technology choices and usages
Failure / trust / security constraints
Verification / acceptance
Whole-system relationships
```

Для Aveli эти вопросы естественно превратились в:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

Для другой системы `backend/` или `frontend/` могут быть неприменимы, а `worker/`, `plugin/`, `gateway/` или `operations/` могут заслуживать top-level ownership.

> **Сделай явной каждую важную system perspective и позволь реальной архитектуре определить её physical owner.**

---

## 7. Business и Technical Knowledge связаны, но разделены

Business documentation отвечает:

```text
Зачем это существует?
Кому это нужно?
Что входит в scope?
Какое behavior должно сохраняться?
Какой результат считается приемлемым?
```

Technical documentation отвечает:

```text
Как это реализовано?
Какой component этим владеет?
Какие data/interfaces участвуют?
Какая technology это поддерживает?
Как это может сломаться?
```

Technical fact попадает в business только если materially меняет product behavior, scope, external contract или user-visible capability.

Предпочтительно:

```text
Account state is server-managed.
Professional workspace data is not synchronized between devices.
```

а не:

```text
Stored in PostgreSQL.
Implemented with NestJS.
```

---

## 8. Ownership Before Detail

До детализации implementation нужно определить owner responsibility.

Для важного behavior задаём вопросы:

```text
Кто принимает решение?
Кто хранит canonical state?
Кто может его менять?
Кто только потребляет?
Кто его проверяет?
```

Только после этого стоит финализировать API, schema, state model, technology или UI behavior.

---

## 9. Data Documentation: от Ownership к Persistence

Data моделируются в dependency order:

```text
Data ownership
    ↓
Conceptual model
    ↓
Logical model
    ↓
Physical persistence model
    ↓
Migrations / constraints / lifecycle
```

System-wide logical model не должен стирать реальные storage boundaries. Physical model принадлежит persistence owner.

Если продукт одновременно использует local SQLite и server PostgreSQL, эти physical models остаются раздельными, даже если logical domain view их связывает.

---

## 10. Technology как Analytical Object

Technology documentation должна отвечать больше, чем «мы используем X».

Полезный technology document объясняет, где известно:

```text
Какую responsibility поддерживает?
Почему используется здесь?
Где используется?
Кто зависит от неё?
Какие ограничения создаёт?
Насколько critical?
Можно ли заменить?
Какие alternatives важны?
```

Technology ownership следует за responsibility, которую technology primarily реализует.

Примеры Aveli:

```text
SQLite / PostgreSQL
→ database persistence technologies

Drift
→ frontend data-access technology

Prisma
→ backend data-access technology

RevenueCat
→ external integration с frontend/backend consumers
```

Contextual usage может появляться в других местах, но canonical technology knowledge не должна дублироваться между perspectives.

---

## 11. Три измерения Technical Knowledge

SSAD разделяет три связанных вопроса.

### Structural knowledge

Что существует?

### Technology knowledge

Что используется?

### Usage knowledge

Где и зачем technology участвует?

Пример:

```text
Drift
→ local repository implementations
→ reactive workspace data
→ local migrations
```

Такое разделение делает human navigation и automated analysis надёжнее.

---

## 12. Internal Interfaces и External Integrations — разные boundaries

Internal interface связывает components внутри анализируемой системы:

```text
Aveli Mobile
↕
Aveli Backend
```

Concrete contracts принадлежат owning component, например `backend/api/`.

External integration пересекает system boundary:

```text
Aveli
↕
RevenueCat / App Store / Google Play / OS service / third-party API
```

Integration documentation владеет cross-boundary relationship: external responsibility, identity mapping, exchanged data, trust, authentication, failure behavior и reconciliation.

Это не позволяет `integrations/` превратиться в общий bucket для каждого API call.

---

## 13. System View как Synthesis Layer

Final `system/` обычно строится после того, как major component perspectives уже понятны.

Он владеет knowledge, которое нельзя назначить одному component:

```text
system context
component relationships
end-to-end flows
cross-component data movement
trust / authority map
system invariants
boundary-changing evolution
whole-system failure scenarios
```

Он не должен становиться второй копией backend/frontend/database/integrations.

> **Слой может ссылаться на другой слой, но модель, описывающая сразу несколько слоёв, принадлежит их ближайшему общему системному уровню.**

Preliminary system sketch может существовать рано. Final system model должен синтезироваться из stabilized lower-level evidence.

---

## 14. Dependency-Driven Construction

Documentation имеет dependencies так же, как implementation.

Полезный default order:

```text
Existing evidence / product reality
        ↓
Business context and scope
        ↓
Rules / requirements / processes
        ↓
Initial responsibility boundaries
        ↓
Data ownership and domain model
        ↓
Interfaces and external constraints
        ↓
Component architecture
        ↓
Technology and implementation evidence
        ↓
System-wide synthesis
        ↓
Traceability / open questions / final consistency review
```

Точный порядок может меняться, если external constraint является upstream.

> **Build knowledge in dependency order and stabilize upstream decisions before detailing downstream implementation.**

---

## 15. Evidence-First Documentation

При анализе существующей реализации сначала собирается evidence:

```text
source code
schemas
API contracts
configuration
native project files
provider contracts
current behavior
tests
existing documentation
stakeholder decisions
```

Нужно разделять:

```text
VERIFIED
INFERRED
OPEN
```

Assumption не должна превращаться в system truth только потому, что так архитектура выглядит аккуратнее. Target-state proposal должен быть явно помечен как target state.

---

## 16. Stabilization и Review

Полезный lifecycle:

```text
Draft
  ↓
Baseline
  ↓
Stable
```

- **Draft** — система ещё исследуется.
- **Baseline** — artifact достаточно стабилен, чтобы стать upstream input для более глубокой работы.
- **Stable** — artifact cross-checked с relevant implementation, contracts или stakeholder decisions и может быть canonical.

Downstream discovery может снова открыть upstream decision. Цель — не исключить iteration, а исключить accidental rework из-за неправильного порядка анализа.

---

## 17. Tasks, Changes и Optional Perspectives

Task не является по своей природе business artifact. Он принадлежит области изменения:

```text
backend
frontend
database
integration
operations
cross-system
```

Task/change directories создаются только при наличии реальных artifacts.

То же относится к optional perspectives вроде `operations/`. Она появляется, когда система действительно имеет самостоятельную operational responsibility: deployment, observability, backups, incident recovery, release operations и т. п.

---

## 18. Traceability как Navigation Between Abstraction Levels

Traceability должна позволять идти и вперёд, и назад:

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

Не каждая trace обязана содержать каждый шаг. Полезные вопросы:

```text
Почему этот component/technology существует?
Как business rule сохраняет смысл в implementation?
Как подтверждено нужное behavior?
```

---

## 19. Human-Readable и Machine-Analyzable Knowledge

Основной repository пишется для людей, но structure должна позволять automated review.

Future metadata может описывать relationships:

```yaml
technology: drift
owner: frontend
used_by:
  - local repositories
supports:
  - local-first workspace
criticality: high
replaceability: medium
```

Это позволяет задавать cross-project questions о technologies, traceability, dependency risk и repeated architecture patterns.

AI может помогать находить contradictions, stale links, uncovered requirements и dependency gaps. Он не заменяет ownership и human-readable explanation.

---

## 20. Bilingual Documentation

Human-readable docs могут поддерживаться парами:

```text
README.md
README.ru.md
```

Обе версии описывают одно knowledge, а не являются независимыми documents.

Machine-readable artifacts остаются общими:

```text
openapi.yaml
schema.sql
PlantUML source
JSON / YAML schemas
```

Implementation identifiers не переводятся.

---

## 21. Чем SSAD не является

SSAD не заменяет UML, BPMN, C4, ADR, OpenAPI, SQL/schema documentation, source code, tests, product management или operational runbooks.

Это способ **организовать и связать эти формы знания вокруг самой системы**.

Методология также не заявляет, что её отдельные практики являются новыми. Она объединяет established documentation practices вокруг system-oriented ownership model и проверяет этот подход на реальных проектах.

---

## 22. Aveli как первый полный Validation Case

Aveli выявил несколько важных corrections:

- обязательные analytical questions важнее обязательных названий папок;
- data ownership нужно стабилизировать до physical schemas и многих API;
- technology ownership следует за responsibility, а не за первым документом, где library упомянута;
- internal API не является автоматически external integration;
- `system/` лучше работает как late synthesis layer;
- failure scenarios и release readiness могут быть system-level review views без обязательного `operations/`;
- legacy artifact-oriented docs нужно удалять только после orphan-knowledge check.

Итоговая структура Aveli:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

с supporting screenshots, rendered diagrams, methodology и repository rules.

Эта структура появилась из архитектуры Aveli и не является template для копирования во все проекты.

---

## Working Definition

> **System-Structured Analysis Documentation — развивающаяся методология документации, в которой analytical и technical knowledge организованы вокруг реальных responsibilities и boundaries системы, а canonical ownership, contextual references, traceability и system-level synthesis связывают business intent с implementation и verification.**

Кратко:

```text
system-shaped hierarchy
+
canonical ownership
+
progressive depth
+
contextual usage
+
evidence-first construction
+
explicit technology modeling
+
traceability
+
system synthesis
+
human-readable knowledge graph
```

Методология должна продолжать развиваться из evidence реальных проектов, а не из стремления к теоретической полноте.
