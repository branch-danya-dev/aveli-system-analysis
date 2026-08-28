# Aveli — Scope

<p align="center">
  <a href="scope.md"><b>English</b></a> ·
  <a href="scope.ru.md">Русский</a>
</p>

> Defines the current product boundary of Aveli: what the system includes, what it intentionally excludes, and which product constraints shape the current version.

---

## Overview

Aveli is a personal mobile workspace for independent beauty professionals who manage their own clients, schedule, services, visits, and payments.

The product is designed around the everyday workflow of one specialist rather than around the management of a large organization.

The current scope combines two product responsibilities:

```text
Professional Workspace
        +
Account & Access
```

The **Professional Workspace** supports the user's daily work.

**Account & Access** controls who can enter the workspace and under which access conditions.

These responsibilities are connected, but they are intentionally treated as different parts of the product.

---

## Product Boundary

Aveli is responsible for helping an individual specialist organize and maintain their own professional activity.

The product includes:

- personal client management;
- appointment planning;
- service and pricing management;
- visit history;
- payment tracking;
- reminders;
- working schedule settings;
- user profile and application preferences;
- account registration and sign-in;
- trial and paid access;
- restoration of previously purchased access.

Aveli is **not** positioned as a full business management platform, enterprise CRM, accounting system, or multi-user collaboration environment.

---

## In Scope

### Account and Access

Aveli supports:

- account registration;
- sign-in;
- session restoration;
- logout;
- trial access;
- recurring subscription access;
- restoration of an existing purchase;
- temporary manual access;
- lifetime access;
- workspace blocking when no valid access source exists.

Access applies to the workspace as a whole.

The current product does not contain separate paid feature tiers.

Loss of access must not remove previously created workspace information.

---

### Today

The Today area provides the specialist with a working view of the current day.

It is intended to answer practical daily questions such as:

- what appointments are planned today;
- what activity is currently relevant;
- which visits require attention;
- what information is needed to continue the working day.

Today is a workspace view, not a separate source of business data.

---

### Calendar

The Calendar provides date-based navigation through appointments.

It allows the specialist to:

- review upcoming appointments;
- navigate through past and future dates;
- understand workload for a selected day;
- open and manage scheduled visits.

The Calendar reflects the current appointment state and is part of the specialist's planning workflow.

---

### Clients

Aveli provides a personal client directory.

The specialist can:

- create and update client records;
- browse and search clients;
- archive and restore clients;
- review client details;
- review available visit history;
- create Aveli client records from device contacts where permission is granted.

Client management is designed for the specialist's own workspace and does not provide shared CRM functionality.

---

### Services

The specialist can maintain the services they provide.

A service may include business information such as:

- name;
- price;
- expected duration;
- active or inactive state.

Services are used when planning appointments and recording completed work.

---

### Appointments and Visits

Aveli supports the appointment lifecycle required for everyday work.

The specialist can:

- create an appointment;
- select a client and service;
- choose date and time;
- reschedule an appointment;
- cancel an appointment;
- mark a no-show;
- complete a visit.

Completed work may include additional visit information such as notes, photos, and payment state.

Scheduling behavior must respect the product's defined appointment rules.

---

### Payments

Aveli allows the specialist to record whether completed work has been paid.

The product supports:

- recording a payment;
- keeping a completed visit in an outstanding state;
- reviewing unpaid visits;
- calculating basic period-based financial information from recorded work.

Aveli does not provide full accounting, taxation, payroll, or financial reporting functionality.

---

### Notes and Photos

The specialist can maintain contextual information related to completed work.

This includes:

- visit notes;
- visit photos.

These materials belong to the user's professional workspace and remain associated with the relevant visit context.

---

### Reminders

Aveli supports reminders related to scheduled appointments.

The current scope focuses on reminders for the specialist using the application.

It does not include a server-driven client communication platform.

---

### Profile and Settings

The specialist can manage personal application settings, including:

- profile information;
- working schedule;
- application language;
- appearance settings;
- currency;
- related personal preferences.

The application supports Russian and English localization.

---

### Data Export and Import

The current scope includes the ability to export and import the user's workspace data.

The exact technical format, validation rules, compatibility model, and migration behavior are documented outside the business layer.

---

### Supported Platforms

Aveli is intended for:

- Android;
- iOS.

Platform-specific implementation details are documented in the frontend and operations areas.

---

## Data Scope

Aveli distinguishes between two categories of information at the product level.

### Professional Workspace Data

This includes information created during the specialist's work:

```text
Clients
Services
Appointments
Visits
Payments
Notes
Photos
Schedule
Workspace preferences
```

The current product model treats this information as part of the user's personal workspace.

It is not synchronized between multiple devices in the current scope.

### Account and Access Data

This includes information required to identify the user and determine whether the workspace may currently be opened:

```text
Account
Session
Trial
Access grants
Subscription state
```

The detailed technical ownership and storage model is documented in:

- [`../../database/`](../../database/)
- [`../../backend/`](../../backend/)
- [`../../frontend/`](../../frontend/)

---

## Offline Scope

The core professional workspace is designed to remain usable without permanent network connectivity.

While offline, the specialist should still be able to continue normal work with locally available workspace information.

Some account-level operations require connectivity, including:

- authentication when a trusted session cannot be restored;
- access verification when previously verified access is no longer sufficient;
- subscription purchase and restoration;
- account-related synchronization.

Offline capability does not mean permanent unrestricted access without account verification.

Detailed behavior belongs to the technical documentation:

- [`../../frontend/offline/`](../../frontend/offline/)
- [`../../backend/access/`](../../backend/access/)

---

## External Capability Scope

Aveli interacts with several external capabilities as part of the product.

These include:

- mobile platform subscription infrastructure;
- device contacts;
- local device notifications;
- an external exchange-rate source.

This section defines only their product role.

Provider-specific APIs, SDKs, security rules, failure handling, and integration architecture are documented in:

[`../../integrations/`](../../integrations/)

---

## Out of Scope

The current product does not include:

- collaborative multi-user workspaces;
- organization or employee management;
- role-based team access;
- synchronization of professional workspace data between devices;
- cloud backup of professional workspace data as a managed service;
- public online booking;
- server-side client CRM;
- server-side appointment management;
- server-driven client messaging;
- marketing automation;
- payroll;
- inventory management;
- full accounting functionality;
- advanced financial reporting;
- store-managed free trial in addition to the Aveli trial model.

These exclusions are intentional parts of the current product boundary.

---

## Product Constraints

### Personal Workspace Model

Aveli is designed around one specialist managing their own professional activity.

Features that require shared ownership, team roles, or organization-level coordination are outside the current model.

### Single Workspace Access Model

A valid access source unlocks the workspace as a whole.

Individual workspace features are not independently paywalled.

### Data Continuity

Expiration of trial or subscription access must not destroy the user's existing professional workspace information.

If access is restored, previously available workspace information should remain available.

### Offline-Oriented Daily Work

Normal professional activity should not depend on permanent network availability.

### No Multi-Device Workspace Synchronization

The current product does not keep professional workspace data synchronized across multiple devices.

This is a product boundary, not merely a missing convenience feature.

---

## Boundary-Changing Features

Some future capabilities would materially change the current scope and require a new system boundary.

Examples include:

```text
Multi-device synchronization
Public online booking
Shared team workspace
Cloud-managed professional data
Server-driven client communication
Organization-level roles
```

Such changes should not be treated as small feature additions.

They would require a review of business scope, data ownership, system responsibilities, and related technical architecture.

---

## Related Documentation

### Business

- [`../context/product-context.md`](../context/product-context.md)
- [`../requirements/functional-requirements.md`](../requirements/functional-requirements.md)
- [`../requirements/non-functional-requirements.md`](../requirements/non-functional-requirements.md)
- [`../requirements/business-rules.md`](../requirements/business-rules.md)
- [`../requirements/acceptance-criteria.md`](../requirements/acceptance-criteria.md)

### Technical

- [`../../database/`](../../database/)
- [`../../backend/`](../../backend/)
- [`../../frontend/`](../../frontend/)
- [`../../integrations/`](../../integrations/)
- [`../../system/`](../../system/)
