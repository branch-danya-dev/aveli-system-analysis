<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&height=220&text=Aveli&fontAlign=50&fontAlignY=38&desc=System%20Analysis%20Case%20%C2%B7%20Local-first%20Mobile%20Workspace&descAlign=50&descAlignY=58&animation=fadeIn&color=gradient&customColorList=12,14,19,20,24&fontColor=fff7f2&descColor=fff7f2" alt="Aveli banner" />
</p>

<p align="center">
  <a href="README.md">English</a> ·
  <a href="README.ru.md"><b>Русский</b></a>
</p>

<p align="center">
  <strong>Кейс системного анализа local-first мобильного рабочего пространства с серверным управлением identity, access и subscription billing.</strong>
</p>

<p align="center">
  <code>Системный анализ</code>
  <code>Local-first</code>
  <code>Владение данными</code>
  <code>REST API</code>
  <code>Offline Access</code>
  <code>Billing Integration</code>
  <code>SSAD</code>
</p>

---

## Что такое Aveli?

**Aveli** — персональное мобильное рабочее пространство для независимых специалистов индустрии красоты.

Приложение поддерживает ежедневную работу с:

- клиентами и историей визитов;
- записями и календарём;
- услугами и ценами;
- оплатами и задолженностями;
- заметками и фотографиями визитов;
- локальными напоминаниями;
- настройками профиля и рабочего пространства.

Продукт намеренно остаётся лёгким. Это не система управления салоном и не server-hosted CRM.

Главная архитектурная граница:

```text
Professional Workspace
        ↓
   User Device

Identity / Access / Billing
        ↓
      Backend
```

Профессиональные данные остаются локальными. Identity аккаунта, sessions, trial, grants и normalized subscription state остаются под контролем backend.

---

## Экраны продукта

<table>
<tr>
<td width="50%" align="center">
  <img src="screenshots/aveli-today-screen.png" alt="Aveli Today" />
  <br><sub><b>Сегодня</b> — текущее расписание и рабочий день</sub>
</td>
<td width="50%" align="center">
  <img src="screenshots/aveli-calendar-screen.png" alt="Aveli Calendar" />
  <br><sub><b>Календарь</b> — записи и планирование дня</sub>
</td>
</tr>
<tr>
<td width="50%" align="center">
  <img src="screenshots/aveli-clients-screen.png" alt="Aveli Clients" />
  <br><sub><b>Клиенты</b> — справочник и история взаимодействий</sub>
</td>
<td width="50%" align="center">
  <img src="screenshots/aveli-another-screen.png" alt="Aveli Workspace" />
  <br><sub><b>Рабочее пространство</b> — дополнительный сценарий продукта</sub>
</td>
</tr>
</table>

---

## Системная проблема

Aveli объединяет две области с принципиально разными требованиями к ownership и availability.

### Профессиональная работа

```text
Клиенты
Услуги
Записи
Оплаты
Заметки
Фотографии
Расписание
Настройки workspace
```

Это operational data, которые должны быть полезны даже при временной недоступности backend.

### Account и коммерческая инфраструктура

```text
Authentication
Sessions
Registration trial
Manual / lifetime grants
Subscription
Billing reconciliation
Workspace access
```

Здесь нужен trusted server-side authority.

Поэтому current architecture не превращает профессиональные данные в synchronized backend domain, пока сама граница продукта не изменится.

---

## Высокоуровневая архитектура

<p align="center">
  <img src="renderer/system-context.svg" alt="Aveli System Context" width="900" />
</p>

```text
Independent Specialist
        ↓
   Aveli Mobile Client
      /            \
     /              \
Local Workspace     Account / Access API
     ↓                    ↓
SQLite + Files        Aveli Backend
                           ↓
                    PostgreSQL
                           ↕
                       RevenueCat
                           ↕
                Apple App Store / Google Play
```

Дополнительные external boundaries: device contacts, local notifications, camera/gallery, OS handoff и exchange-rate API.

Полный cross-layer view находится в [`system/`](system/).

---

## Ключевые системные решения

### 01 · Local-first workspace

Обычные профессиональные операции работают напрямую с local persistence.

```text
User action
   ↓
Frontend domain/repository
   ↓
Drift / SQLite + local files
```

Cloud copy клиентов, записей, оплат, заметок и фотографий для обычной работы не требуется.

### 02 · Изоляция workspace по пользователю

Authenticated backend identity выбирает local workspace:

```text
users.id
   ↓
aveli_<userId>.sqlite
```

Visit photos и cached access snapshot используют ту же user-specific boundary.

### 03 · Backend-controlled access

Backend выбирает один effective source:

```text
Lifetime
   ↓
Manual Grant
   ↓
Subscription
   ↓
Trial
   ↓
None
```

Frontend использует resolved result и не вычисляет entitlement precedence самостоятельно.

### 04 · Access не владеет данными

```text
Access expired
      ≠
Delete workspace
```

Окончание trial/subscription может заблокировать доступность workspace, но не удаляет local professional data.

### 05 · Контролируемое offline trust

Ранее проверенный access state хранится в secure storage.

```text
Backend verification
      ↓
Trusted snapshot
      ↓
Temporary offline authorization
      ↓
Verification required again
```

Server-provided verification deadline имеет приоритет. В current client также существует 72-hour implementation default для соответствующего случая.

### 06 · Purchase не равен прямому workspace access

```text
Store purchase
      ↓
RevenueCat
      ↓
POST /v1/billing/sync
      ↓
Backend reconciliation
      ↓
AccessStatusView
      ↓
Frontend Access Gate
```

Client-side RevenueCat result не обходит access model Aveli backend.

### 07 · Logout не равен profile deletion

Logout очищает active identity/access context и закрывает local database, сохраняя professional workspace.

Explicit profile deletion — отдельный destructive lifecycle.

---

## Архитектура документации

Репозиторий организован по рабочей методологии **System-Structured Analysis Documentation (SSAD)**.

> **Документация отражает систему.**

Knowledge принадлежит части системы, которая реально владеет соответствующей responsibility:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

Supporting areas:

```text
screenshots/
renderer/
methodology.md
rules.md
```

| Область | Canonical responsibility |
|---|---|
| [`business/`](business/) | Product context, scope, requirements, rules, processes и traceability. |
| [`database/`](database/) | Data ownership, conceptual/logical models и physical persistence. |
| [`backend/`](backend/) | Account, authentication, access, billing, API и server behavior. |
| [`frontend/`](frontend/) | Flutter runtime, navigation, state, local workspace, offline behavior и device usage. |
| [`integrations/`](integrations/) | RevenueCat, stores, device capabilities и third-party boundaries. |
| [`system/`](system/) | Cross-component context, flows, trust, invariants, evolution и review. |
| [`methodology.ru.md`](methodology.ru.md) | Почему SSAD устроен именно так. |
| [`rules.ru.md`](rules.ru.md) | Нормативные правила документации. |

> **Storage is hierarchical. Knowledge is graph-based.**

---

## Рекомендуемые пути чтения

### Быстрый обзор системы

```text
README
  ↓
business/README
  ↓
system/README
  ↓
system/architecture
  ↓
system/flows
  ↓
system/invariants
```

### Основные canonical areas

- Product и requirements → [`business/`](business/)
- Data ownership и persistence → [`database/`](database/)
- Backend contracts и access → [`backend/`](backend/)
- Client runtime и local workspace → [`frontend/`](frontend/)
- External providers и device boundaries → [`integrations/`](integrations/)
- Whole-system synthesis → [`system/`](system/)

### Whole-system review

- [`system/trust/`](system/trust/)
- [`system/invariants/`](system/invariants/)
- [`system/evolution/`](system/evolution/)
- [`system/review/failure-scenarios.ru.md`](system/review/failure-scenarios.ru.md)
- [`system/review/release-readiness.ru.md`](system/review/release-readiness.ru.md)
- [`system/review/open-questions.ru.md`](system/review/open-questions.ru.md)

---

## Технологический стек

Стек показан по responsibilities, а не как плоский список dependencies.

<table>
<tr>
<td width="50%" valign="top">

### Mobile Application

<p>
  <img src="https://img.shields.io/badge/Flutter-02569B?style=flat-square&logo=flutter&logoColor=white" alt="Flutter">
  <img src="https://img.shields.io/badge/Dart-0175C2?style=flat-square&logo=dart&logoColor=white" alt="Dart">
  <img src="https://img.shields.io/badge/Riverpod-0D1117?style=flat-square" alt="Riverpod">
  <img src="https://img.shields.io/badge/go__router-02569B?style=flat-square" alt="go_router">
</p>

```text
Flutter / Dart
Riverpod
go_router
```

Владеет:

```text
UI
Navigation
Application state
Local workspace behavior
Access Gate
Offline client behavior
```

</td>
<td width="50%" valign="top">

### Local Data & Device

<p>
  <img src="https://img.shields.io/badge/Drift-4B5563?style=flat-square" alt="Drift">
  <img src="https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white" alt="SQLite">
  <img src="https://img.shields.io/badge/Secure%20Storage-111827?style=flat-square" alt="Secure Storage">
  <img src="https://img.shields.io/badge/Local%20Notifications-374151?style=flat-square" alt="Local Notifications">
</p>

```text
Drift
SQLite
flutter_secure_storage
flutter_local_notifications
Local filesystem
```

Отвечает за:

```text
Professional workspace persistence
Per-user isolation
Visit-photo files
Trusted access snapshot
Local reminders
```

</td>
</tr>

<tr>
<td width="50%" valign="top">

### Backend

<p>
  <img src="https://img.shields.io/badge/NestJS-E0234E?style=flat-square&logo=nestjs&logoColor=white" alt="NestJS">
  <img src="https://img.shields.io/badge/Prisma-2D3748?style=flat-square&logo=prisma&logoColor=white" alt="Prisma">
  <img src="https://img.shields.io/badge/JWT-000000?style=flat-square&logo=jsonwebtokens&logoColor=white" alt="JWT">
  <img src="https://img.shields.io/badge/Argon2id-111827?style=flat-square" alt="Argon2id">
</p>

```text
NestJS
Prisma
JWT access tokens
Rotating refresh tokens
Argon2id
REST API
```

Владеет:

```text
Identity
Authentication
Sessions
Trial / grants
Access resolution
Billing reconciliation
```

</td>
<td width="50%" valign="top">

### Server Data

<p>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/REST%20API-0D1117?style=flat-square" alt="REST API">
  <img src="https://img.shields.io/badge/OpenAPI-6BA539?style=flat-square&logo=openapiinitiative&logoColor=white" alt="OpenAPI">
</p>

```text
PostgreSQL
Prisma migrations
OpenAPI
```

Хранит:

```text
Users
Auth sessions
Access grants
Subscriptions
Subscription events
```

</td>
</tr>

<tr>
<td width="50%" valign="top">

### External Integrations

<p>
  <img src="https://img.shields.io/badge/RevenueCat-F25A5A?style=flat-square" alt="RevenueCat">
  <img src="https://img.shields.io/badge/App%20Store-0D96F6?style=flat-square&logo=appstore&logoColor=white" alt="App Store">
  <img src="https://img.shields.io/badge/Google%20Play-414141?style=flat-square&logo=googleplay&logoColor=white" alt="Google Play">
</p>

```text
RevenueCat
Apple App Store
Google Play
Device Contacts
OS Notifications
Camera / Gallery
Exchange Rate API
OS handoff
```

</td>
<td width="50%" valign="top">

### Analysis & Documentation

<p>
  <img src="https://img.shields.io/badge/PlantUML-Diagrams-0D1117?style=flat-square" alt="PlantUML">
  <img src="https://img.shields.io/badge/OpenAPI-Contracts-6BA539?style=flat-square&logo=openapiinitiative&logoColor=white" alt="OpenAPI">
  <img src="https://img.shields.io/badge/Markdown-000000?style=flat-square&logo=markdown&logoColor=white" alt="Markdown">
  <img src="https://img.shields.io/badge/SSAD-Documentation-4B5563?style=flat-square" alt="SSAD">
</p>

```text
Markdown
PlantUML
OpenAPI
SSAD
GitHub
```

Используется для:

```text
Requirements
Architecture
Data models
API contracts
Traceability
System synthesis
```

</td>
</tr>
</table>

Canonical technology ownership и rationale находятся в соответствующих `stack/` и integration areas.

---

## API Surface

Backend API намеренно узкий: professional workspace entities не являются server-owned.

Ключевые contracts:

```text
/v1/auth/*
GET  /v1/access
POST /v1/billing/sync
POST /v1/webhooks/revenuecat
/health
/ready
```

Canonical API documentation: [`backend/api/`](backend/api/)

---

## Владение данными

Ключевое различие — не только entity relationships, но и ownership.

```text
LOCAL PROFESSIONAL WORKSPACE
Client
Service
Appointment
Payment
Visit Note
Visit Photo
Workspace Settings

SERVER IDENTITY / ACCESS
User
Auth Session
Access Grant
Subscription
Subscription Event
```

Canonical data architecture: [`database/`](database/)

---

## Failure и Release Model

Aveli использует cross-system isolation principle:

```text
Technical Failure
      ≠
Delete User Work
```

Backend outages, billing failures, unavailable stores, denied permissions или exchange-rate failures должны быть изолированы от unrelated local professional data.

Production readiness рассматривается как whole-system concern: mobile config, backend secrets, migrations, store products, RevenueCat mapping и Access Gate должны быть согласованы.

См.:

- [`system/review/failure-scenarios.ru.md`](system/review/failure-scenarios.ru.md)
- [`system/review/release-readiness.ru.md`](system/review/release-readiness.ru.md)

---

## Набор диаграмм

Rendered diagrams находятся в [`renderer/`](renderer/):

```text
system-context.svg
component-model.svg
data-model.svg
access-sequence.svg
access-state-machine.svg
integration-sequence.svg
user-flow.svg
```

Machine-maintainable diagram sources остаются рядом с analytical documents, где это применимо.

---

## Методология

Этот репозиторий — первый полный validation case рабочего подхода SSAD.

SSAD делает акцент на:

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
late system synthesis
```

Читайте:

- [`methodology.ru.md`](methodology.ru.md)
- [`rules.ru.md`](rules.ru.md)

SSAD не позиционируется как замена UML, BPMN, C4, ADR, OpenAPI или docs-as-code. Это развивающийся способ организовать и связать эти формы знаний вокруг реальной системы.

---

## Текущий статус

Analytical baseline завершён:

```text
business/      Stable
database/      Stable
backend/       Stable
frontend/      Stable
integrations/  Stable
system/        Stable
```

Final whole-system review закрыл structural и technology-ownership issues, а оставшиеся non-blocking items классифицированы как accepted limitations, external release evidence, future product decisions или architecture-change triggers.

Closure register:

[`system/review/open-questions.ru.md`](system/review/open-questions.ru.md)

Repository отделяет стабильную architecture baseline от фактов, которым в будущем понадобится provider/device evidence или новое product decision.

---

## Контакты

<p align="center">
  <strong>Daniel Rogulin</strong><br>
  System Analyst · Software / System Design
</p>

<p align="center">
  <a href="https://github.com/branch-danya-dev">
    <img src="https://img.shields.io/badge/GitHub-branch--danya--dev-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub">
  </a>
</p>

По вопросам системного анализа, архитектурных решений или самого кейса Aveli можно связаться со мной напрямую.

---

## Результат

Aveli демонстрирует системный анализ через:

```text
Business rules
+
Data ownership
+
Local-first architecture
+
REST contracts
+
Authentication and sessions
+
Workspace entitlement
+
External billing
+
Offline trust
+
Device integrations
+
Failure isolation
+
Release readiness
+
Cross-layer synthesis
```

Результат — одновременно реализованный product case и structured technical knowledge base, сохраняющий смысл аналитических решений при переходе к реальному коду.

---

<p align="center">
  <strong>System analysis designed to survive implementation.</strong>
</p>
