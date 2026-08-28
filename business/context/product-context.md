# Aveli — Product Context

<p align="center">
  <a href="product-context.md"><b>English</b></a> ·
  <a href="product-context.ru.md">Русский</a>
</p>

> Describes why Aveli exists, who it is designed for, which user problems it addresses, and how the product is positioned.

---

## Overview

Aveli is a personal mobile workspace for independent beauty professionals who manage their own clients, appointments, services, visits, and payments.

The product is designed for specialists who need a practical everyday tool rather than a large CRM with complex organizational features.

Aveli focuses on one core idea:

> **Give an independent specialist one calm, focused workspace for everyday professional activity.**

The product should help the user organize work without turning routine actions into administrative overhead.

---

## Target Users

Aveli is primarily designed for independent specialists who manage their own client flow and working schedule.

Typical users include:

- manicure specialists;
- brow artists;
- hair specialists;
- lash specialists;
- makeup artists;
- other independent beauty professionals;
- small private studios where one person manages their own work.

The common characteristic is not a specific beauty specialization.

It is the operating model:

```text
One specialist
    ↓
Own clients
    ↓
Own schedule
    ↓
Own services
    ↓
Own payments
```

The product is built around this personal-workspace model.

---

## User Context

Independent specialists often manage their work across several disconnected tools.

A typical workflow may involve:

- a phone calendar for appointments;
- contacts or messaging apps for client information;
- notes for visit history;
- spreadsheets or memory for payments;
- photo galleries for visit results;
- separate reminders;
- manual tracking of unpaid visits.

This creates fragmentation.

The user may know all the required information, but it is distributed across different places and is difficult to maintain consistently.

Aveli brings these everyday activities into one workspace.

---

## User Problems

### Fragmented Information

Client details, appointments, payments, notes, and photos may exist in different applications.

This increases the amount of manual work required to keep information consistent.

### Repetitive Daily Administration

Simple actions such as checking today's schedule, reviewing a client's history, or identifying an unpaid visit should not require several tools.

### Heavy CRM Complexity

Many business-management products are designed for salons, teams, administrators, marketing, inventory, and large operational processes.

For an independent specialist, this may introduce more complexity than value.

### Dependence on Connectivity

A specialist may need access to their working information even when network connectivity is unstable or unavailable.

### Loss of Work Context

A simple list of appointments is often not enough.

The specialist may also need:

- previous visit information;
- notes;
- photos;
- payment state;
- service information.

Aveli treats these elements as part of one professional context.

---

## Product Goal

The main goal of Aveli is to provide a **complete but lightweight personal workspace** for everyday professional activity.

The product should allow the specialist to move through the working day without constantly switching between unrelated tools.

The desired experience is:

```text
Open Aveli
    ↓
Understand the day
    ↓
Manage appointments
    ↓
Work with clients
    ↓
Complete visits
    ↓
Record payment and context
    ↓
Continue the workflow
```

The application should support the user's work rather than become another process the user has to manage.

---

## Product Positioning

Aveli is positioned as a lightweight professional workspace.

It is closer to:

```text
Personal work organizer
+
Client workspace
+
Appointment manager
+
Visit history
+
Simple payment tracking
```

than to:

```text
Enterprise CRM
Salon management platform
Accounting system
ERP
Marketing automation platform
```

This distinction is important.

Aveli intentionally prioritizes:

- simple daily workflows;
- low cognitive load;
- quick access to relevant information;
- continuity of professional context;
- personal ownership of the workspace;
- predictable access behavior.

The product is not intended to reproduce every capability of a large CRM.

---

## Core User Needs

Aveli should give the specialist one place to:

### Plan Work

The user needs to understand what is scheduled and when.

This includes:

- today's activity;
- future appointments;
- calendar navigation;
- rescheduling and cancellation;
- awareness of available working time.

### Manage Clients

The user needs a personal client directory that preserves relevant professional context.

This includes:

- client details;
- visit history;
- notes;
- photos;
- previous services.

### Manage Services

The specialist needs to maintain the services they provide, including their expected duration and price.

### Complete Visits

The workflow should continue beyond the appointment itself.

After a visit, the specialist may need to preserve:

- completion state;
- notes;
- photos;
- payment information.

### Track Payments

The user needs to understand whether completed work has been paid and which visits remain outstanding.

### Keep Working Context Available

The specialist should be able to continue normal work without permanent dependence on network availability.

### Manage Access Predictably

Trial, subscription, and other supported access models should affect the ability to use the workspace without creating uncertainty about the user's existing professional information.

---

## Product Experience Principles

### Focused

Aveli should prioritize the workflows used during normal daily work.

Features that do not support the core professional workflow should not dominate the product.

### Calm

The interface and product behavior should reduce cognitive load rather than create additional pressure.

The workspace should feel predictable and understandable.

### Personal

The product is centered around the individual specialist and their own professional activity.

### Continuous

Appointments, clients, visits, payments, notes, and photos should form one connected work context rather than independent feature islands.

### Resilient

Temporary connectivity or access-related problems should not turn into loss of professional information.

### Transparent

The user should understand:

- whether access is available;
- why access is unavailable;
- what happens to existing information;
- what actions can restore access.

---

## Main Product Areas

At a high level, the product experience is organized around:

```text
Today
Calendar
Clients
More
```

These areas provide access to the main professional workflows:

- daily planning;
- appointment management;
- client management;
- services;
- payments;
- schedule;
- profile;
- reminders;
- application settings;
- access and subscription management.

Detailed product boundaries are defined in:

[`../scope/scope.md`](../scope/scope.md)

---

## Product Differentiation

Aveli's main differentiation is not a single feature.

It is the combination of several product decisions:

```text
Personal workspace
        +
Low-complexity workflow
        +
Connected client / visit context
        +
Offline-oriented daily work
        +
Predictable access model
```

The product is intentionally narrower than a traditional CRM.

That narrower focus is part of the product strategy rather than a temporary limitation.

---

## Success Criteria

At the product level, Aveli is successful when an independent specialist can use it as the primary workspace for normal daily activity without needing a more complex business-management platform.

A successful experience means the user can:

- understand the current working day;
- maintain client information;
- manage appointments;
- preserve visit context;
- track payment state;
- continue working with minimal administrative overhead;
- understand access conditions without risking loss of existing work information.

Detailed measurable requirements and acceptance conditions are documented separately.

---

## Product Evolution

Future product development should be evaluated against the current positioning.

A proposed feature should answer:

```text
Does this improve the personal workflow of an independent specialist?
```

If a feature changes the product toward:

- shared team workspaces;
- organization management;
- public booking infrastructure;
- cloud CRM;
- advanced accounting;
- employee operations;

then it may represent a change in product model rather than a normal extension of the existing workspace.

Such changes require a review of scope and system responsibilities.

---

## Related Documentation

- [`../scope/scope.md`](../scope/scope.md) — current product boundary
- [`../requirements/`](../requirements/) — required product behavior
- [`../processes/`](../processes/) — user and business workflows
- [`../../system/`](../../system/) — system-level technical view
