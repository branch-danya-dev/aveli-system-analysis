# System-Structured Analysis Documentation

> **Рабочее название:** System-Structured Analysis Documentation (SSAD)  
> **Статус:** развивающаяся методология  
> **Назначение:** описать идею, структуру и философию подхода к документации в этом репозитории.

---

## Что это за документ

`methodology.ru.md` объясняет **суть подхода к документации**.

Это не набор обязательных правил.

Правила репозитория определены отдельно:

[`rules.ru.md`](rules.ru.md)

Разница:

```text
methodology.ru.md
    ↓
Зачем существует подход
Как он должен работать
Какую проблему решает
Как связаны его части

rules.ru.md
    ↓
Каким требованиям должны соответствовать документы
Что допустимо
Что недопустимо
Как проверяется консистентность
```

Методология может развиваться по мере роста репозитория и появления новых проектов, которые выявляют слабые или отсутствующие концепции.

---

## Основная идея

Центральная идея проста:

> **Документация системного анализа должна повторять структуру анализируемой системы.**

Традиционные аналитические репозитории часто организованы по типам артефактов:

```text
requirements/
uml/
api/
database/
security/
```

Это удобно автору документов, но не всегда соответствует тому, как саму систему понимают люди, которые ее разрабатывают и поддерживают.

SSAD вместо этого организует знания вокруг реальных ответственностей системы:

```text
business/
database/
backend/
frontend/
integrations/
operations/
system/
```

Конкретная структура не является жесткой.

Иерархия должна следовать реальной системе.

---

## Зачем нужен такой подход

Система не воспринимается как набор аналитических артефактов.

Backend-разработчик мыслит сервисами, API, authentication, billing, messaging, errors и инфраструктурой.

Frontend-разработчик мыслит экранами, state, navigation, local storage, offline behavior и integrations.

Специалист по данным мыслит ownership, schemas, entities, constraints, migrations и lifecycle.

Системный аналитик должен понимать все эти представления.

Поэтому документация должна позволять каждому участнику войти в репозиторий через ту часть системы, которая ему нужна, и при этом иметь возможность перейти к полной картине.

---

## Документация как модель системы

Сам репозиторий становится моделью системы.

На верхнем уровне:

```text
Система
  ↓
Крупная ответственность
  ↓
Компонент
  ↓
Технология
  ↓
Поведение
  ↓
Детали реализации
```

Глубину выбирает читатель.

Так появляется **progressive depth**.

Новому участнику может быть достаточно:

```text
business/
system/
```

Backend-разработчик может продолжить в:

```text
backend/
backend/api/
backend/auth/
backend/stack/
```

Специалист по данным может начать непосредственно с:

```text
database/
```

Репозиторий должен поддерживать все эти пути и не требовать одного обязательного линейного порядка чтения.

---

## Иерархия и Knowledge Graph

У репозитория одновременно существуют две структуры.

### Физическая структура

Файлы и директории образуют иерархию.

```text
backend/
└── auth/
    ├── README.md
    ├── sessions.md
    └── token-lifecycle.md
```

Она удобна для навигации и ownership.

### Структура знаний

Реальные знания о системе не являются иерархическими.

Authentication может зависеть от:

```text
business requirements
database session entities
backend auth logic
frontend bootstrap
security constraints
architecture decisions
```

Поэтому документы должны ссылаться на связанные знания.

Ключевой принцип:

> **Хранение иерархическое. Знания графовые.**

Дерево директорий помогает найти информацию.

Cross-references объясняют реальное устройство системы.

---

## Canonical Knowledge

У каждой важной концепции должно быть одно каноническое место.

Примеры:

```text
backend/api/
    → canonical API contracts

database/
    → canonical data model

backend/stack/kafka/
    → canonical описание Kafka в backend

business/requirements/
    → canonical business requirements
```

Другие документы ссылаются на canonical source вместо повторения его содержания.

Отсюда второй базовый принцип:

> **Не дублируем знания. При необходимости дублируем контекст.**

Например:

```text
backend/stack/kafka/
```

может объяснять:

- почему используется Kafka;
- какую роль она играет в backend;
- какие существуют альтернативы;
- какие зависимости она создает.

А:

```text
backend/logs/
```

описывает:

- как logging использует Kafka;
- какие события передаются;
- какие failure scenarios важны.

Второй документ не переопределяет Kafka.

Он описывает ее в контексте logging.

---

## Stack как самостоятельный объект анализа

Выбор технологий не рассматривается как случайная implementation detail.

Stack является отдельным аналитическим измерением.

Недостаточно написать:

```text
"Используем Redis."
```

Документация должна со временем позволять ответить:

```text
Почему Redis?
Где он используется?
Какие компоненты от него зависят?
Какое требование он поддерживает?
Что сломается при его удалении?
Можно ли его заменить?
Какие альтернативы рассматривались?
Какую operational cost он создает?
```

Поэтому stack явно документируется внутри технических областей.

Пример:

```text
backend/
└── stack/
    ├── redis/
    ├── kafka/
    ├── rest-api/
    └── docker/
```

После этого технология появляется в других местах только через contextual usage там, где реально применяется.

---

## Три измерения технических знаний

SSAD рассматривает технические знания в трех связанных измерениях.

### 1. Structural Knowledge

Что существует в системе?

```text
backend
auth
billing
logs
notifications
database
frontend
```

### 2. Technology Knowledge

Какие технологии используются?

```text
NestJS
PostgreSQL
Redis
Kafka
Flutter
Drift
Docker
```

### 3. Usage Knowledge

Где и зачем каждая технология участвует?

```text
Kafka
  → logs
  → audit
  → notifications

Redis
  → cache
  → session support

Drift
  → local workspace persistence
```

Эта модель важна как для людей, так и для будущего автоматизированного анализа.

---

## Граница Business и Technical

Business и technical documentation должны быть связаны, но разделены.

Business отвечает:

```text
Зачем это существует?
Что нужно пользователю?
Что входит в scope?
Какое поведение должно оставаться истинным?
Какой результат считается приемлемым?
```

Technical documentation отвечает:

```text
Как это реализовано?
Какой компонент этим владеет?
Какая технология используется?
Какие данные участвуют?
Какие интерфейсы задействованы?
Как это ломается?
Как это эксплуатируется?
```

Business statement может иметь технические последствия.

Пример:

```text
Business:
Профессиональные данные workspace не синхронизируются между устройствами.

Technical consequences:
→ frontend/storage/
→ database/local/
→ system/decisions/
```

Business document определяет product truth.

Technical documents объясняют, как система сохраняет эту истину.

---

## Business — не техническое summary

Business document не должен становиться сокращенной архитектурой.

Например:

```text
Stored in PostgreSQL
Uses JWT
Implemented with NestJS
```

не относится к business, если сама технология не имеет прямого product-level последствия.

Предпочтительно:

```text
Account state is server-managed.
Access контролируется на уровне workspace.
Профессиональные данные не синхронизируются между устройствами.
```

После этого дается ссылка на technical implementation.

Так business остается читаемым, а traceability сохраняется.

---

## Технические области — не Artifact Buckets

Техническая директория — не просто место для хранения файлов определенного формата.

Например:

```text
backend/api/
```

владеет реальными API contracts.

```text
backend/stack/rest-api/
```

владеет REST как архитектурным и технологическим решением.

Эти области связаны, но не взаимозаменяемы.

Аналогично:

```text
database/
```

владеет data architecture.

Это не просто папка для ER diagrams.

Она может содержать:

```text
architecture
stack
entities
schemas
constraints
indexes
migrations
queries
ownership
```

Принцип организации — responsibility, а не file format.

---

## Cross-Cutting Views

Некоторые знания не принадлежат одному компоненту.

Например:

```text
system architecture
technology map
dependency graph
architecture decisions
traceability
delivery changes
```

Это cross-cutting views.

Они должны связывать component-level knowledge, а не дублировать его.

Например:

```text
system/
```

может показать, как backend, frontend, database и integrations образуют одну систему.

Но он не должен становиться второй копией всей component documentation.

---

## Traceability как навигация между уровнями абстракции

Traceability — это не только:

```text
Requirement → Test
```

В SSAD целевая цепочка шире:

```text
Business Need
    ↓
Business Rule
    ↓
Requirement
    ↓
Architecture Decision
    ↓
Component
    ↓
Technology / Interface / Data
    ↓
Acceptance
    ↓
Verification
```

Не каждый trace обязан содержать каждый уровень.

Важно, чтобы значимые решения можно было читать в обе стороны.

### Вперед

```text
Зачем системе это нужно?
    ↓
Как это реализовано?
```

### Назад

```text
Зачем существует эта технология или компонент?
    ↓
Какая продуктовая потребность их оправдывает?
```

Второе направление особенно важно при архитектурном review.

---

## Tasks и Change Artifacts

Task сам по себе не является бизнес-артефактом.

Задачи могут существовать в любой области:

```text
backend
frontend
database
operations
integration
```

Task принадлежит контексту изменения, которое описывает.

Поэтому task-directories не должны автоматически создаваться в каждом слое.

Они появляются только тогда, когда проекту действительно нужна task-level documentation.

На business-уровне scope, requirements, business rules, processes и acceptance criteria уже определяют, чего должен достичь продукт.

Дополнительный универсальный слой `business/tasks/` обычно только дублировал бы этот смысл.

---

## Документация должна поддерживать реальную реализацию

SSAD должен выходить за пределы абстрактных аналитических описаний.

Где это уместно, technical documentation должна содержать или ссылаться на реальные артефакты:

```text
OpenAPI
SQL
DDL
Prisma schema
Drift tables
JSON examples
configuration examples
Docker files
migration scripts
code fragments
PlantUML
```

Цель — не превратить документацию в source code.

Цель — уменьшить разрыв между аналитическим намерением и реальной системой.

---

## Документация для разных ролей

Один репозиторий должен быть полезен разным читателям.

### Product / Business

```text
business/
```

### System Analyst

```text
business/
system/
database/
backend/
frontend/
integrations/
```

### Backend Developer

```text
backend/
database/
integrations/
```

### Frontend Developer

```text
frontend/
backend/api/
database/local/
```

### DevOps / Operations

```text
operations/
system/
backend/deployment/
```

### QA

```text
business/requirements/
business/traceability/
operations/testing/
technical component behavior
```

Методология не скрывает от роли остальные знания.

Она дает понятную точку входа и возможность углубляться при необходимости.

---

## Human-Readable и Machine-Readable Knowledge

Репозиторий проектируется одновременно для людей и автоматизированного анализа.

Human-readable documentation объясняет смысл и контекст.

Machine-readable metadata в будущем может описывать связи.

Пример:

```yaml
technology: redis
layer: backend
category: cache

used_by:
  - sessions
  - access

supports:
  - NFR-036

criticality: medium
replaceable: true
```

Это позволит задавать автоматические вопросы:

```text
Где используется Redis?
Какие проекты зависят от Kafka?
Какие технологии регулярно создают operational complexity?
У каких requirements нет acceptance criteria?
У каких компонентов не описаны failure scenarios?
Какие architecture decisions повторяются между проектами?
```

Методология изначально проектируется так, чтобы AI-инструменты могли анализировать структуру репозитория, не заменяя human-readable documentation.

---

## Multi-Project Analysis

Одна из долгосрочных целей — сделать документацию разных проектов сравнимой.

Если несколько репозиториев используют одну концептуальную модель, их можно анализировать совместно.

Например:

```text
Project A использует Kafka для logs и audit.
Project B использует Kafka только для notifications.
Project C вообще не использует broker.

Почему?

Какие требования оправдывали выбор?

Стоила ли operational cost результата?
```

Поэтому methodology рассматривает architecture decisions, technology usage, ownership, criticality и traceability как потенциально анализируемые данные.

Это не обязательно для первой версии проекта.

Но структура должна позволять прийти к этому позже.

---

## Двуязычная документация

Human-readable project documentation поддерживается на английском и русском.

Это не два независимых набора документации.

Они представляют одни и те же знания на двух языках.

Machine-readable artifacts остаются общими, если перевод создавал бы бессмысленное дублирование.

Пример:

```text
README.md
README.ru.md

openapi.yaml       ← общий
schema.sql         ← общий
dependencies.yaml  ← общий
```

---

## Чем SSAD не является

SSAD не предназначен для замены:

- UML;
- BPMN;
- C4;
- OpenAPI;
- ADR;
- SQL documentation;
- source code;
- testing documentation;
- operational runbooks;
- product management.

Это способ **организовать и связать эти формы знаний вокруг самой системы**.

Также методология не требует одинаковых директорий во всех проектах.

Revit plugin, mobile application и distributed banking platform не должны иметь одинаковое физическое дерево документации, если их архитектуры различаются.

---

## Рабочее определение

Текущее рабочее определение:

> **System-Structured Analysis Documentation — методология документации, в которой аналитические и технические знания организуются вокруг реальной структуры и ответственностей системы, а cross-references связывают бизнес-смысл, архитектуру, технологии, implementation context и verification в навигируемый knowledge graph.**

Методология ставит в приоритет:

```text
system-oriented hierarchy
+
progressive depth
+
canonical knowledge
+
contextual usage
+
explicit technology modeling
+
traceability
+
real technical artifacts
+
human and machine readability
```

---

## Развитие

Методология намеренно не считается завершенной.

Новые проекты могут выявить:

- недостающие уровни абстракции;
- слабые границы;
- ненужное дублирование;
- более удачные названия;
- лучшие metadata models;
- новые возможности cross-project analysis.

Тогда процесс выглядит так:

```text
Observation
    ↓
Methodology change
    ↓
Rule update if necessary
    ↓
Repository structure evolves
```

Методология должна развиваться из реального опыта проектов, а не из попытки заранее описать абсолютно все.
