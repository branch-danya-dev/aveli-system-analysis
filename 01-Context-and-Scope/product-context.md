# Aveli — Product Context

## Purpose

Aveli is a mobile workspace for independent beauty professionals who manage appointments, clients, services and payments.

The product is designed as a lightweight alternative to a traditional CRM: focused on daily work, simple interaction and local ownership of operational data.

---

## Target User

Aveli is intended for:

- independent beauty professionals;
- small private studios;
- specialists working with repeat clients;
- users who manage their schedule and payments themselves.

Typical domains include:

- manicure;
- eyebrows;
- hair;
- related personal services.

---

## Core User Needs

The user needs one place to:

- manage clients;
- plan appointments;
- track services and prices;
- record payments;
- review unpaid visits;
- maintain visit notes and photos;
- receive local reminders;
- manage their working schedule.

---

## Product Positioning

Aveli is not designed as a heavy business CRM.

The product focuses on:

- simple daily workflow;
- low cognitive load;
- offline availability of working data;
- privacy of client information;
- clear access and subscription behavior;
- calm visual interaction.

---

## Main Workspace

The application is organized around four primary areas:

```text
Today
  ↓
Calendar
  ↓
Clients
  ↓
More

Supporting functions include:

appointments;
services;
payments;
profile;
schedule settings;
reminders;
appearance;
subscription and access management.
Product Boundary

Aveli contains two different system responsibilities.

Workspace

Handles the user's operational data:

clients;
appointments;
services;
payments;
visit notes;
visit photos;
schedule;
settings.

This data is stored locally on the device.

Account and Access

Handles:

registration and authentication;
trial period;
subscription status;
access grants;
billing synchronization.

This data is handled through the backend.

High-Level Concept
Beauty Professional
        ↓
      Aveli
   /          \
Workspace    Account / Access
   ↓              ↓
Local Data      Backend

This separation is one of the core architectural principles of the product.

Product Goal

The goal is to provide a complete everyday workspace while keeping operational client data local and using the server only for account, access and billing responsibilities.

Summary

Aveli combines:

mobile scheduling;
client management;
payment tracking;
offline-first operational data;
server-controlled access;
subscription integration.

The result is a lightweight professional workspace rather than a traditional cloud CRM.