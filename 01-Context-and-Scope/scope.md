# Aveli — Scope

## Purpose

This document defines the current system boundary of Aveli.

The scope covers the mobile workspace, account and access management, billing integration, local data storage and supporting platform behavior.

---

## In Scope

Aveli includes:

- user registration and sign-in;
- server-controlled trial access;
- subscription-based access through RevenueCat;
- local client management;
- appointment scheduling;
- services and pricing;
- payment tracking;
- visit notes and photos;
- local reminders;
- profile and schedule settings;
- local data export / import;
- RU / EN localization;
- Android and iOS mobile clients.

---

## Core Functional Areas

```text
Account & Access
      │
      ├── Auth
      ├── Trial
      ├── Subscription
      └── Access Gate

Workspace
      │
      ├── Today
      ├── Calendar
      ├── Clients
      ├── Appointments
      ├── Services
      ├── Payments
      └── Settings

      Data Scope

Aveli intentionally separates two data domains.

Server Data

Stored in PostgreSQL:

users;
sessions;
access grants;
subscriptions.
Local Work Data

Stored in a per-user SQLite database:

clients;
services;
appointments;
payments;
visit notes;
visit photos;
schedule;
local settings.

Work data is not synchronized with the backend.

Access Scope

The application supports several access sources:

lifetime access;
manual access grant;
active store subscription;
active 30-day trial;
no access.

The workspace is either available as a whole or blocked through a single Access Gate.

Individual features are not separately paywalled.

Integration Scope

Aveli currently integrates with:

Aveli backend API;
RevenueCat;
Google Play / App Store subscription infrastructure;
device secure storage;
device contacts;
local notification services;
external exchange-rate source.
Offline Scope

Core workspace data is designed for local use.

A user with a valid cached access state can continue working during a limited offline period.

Offline behavior includes:

local clients and appointments;
local payments and notes;
local photos;
local reminders;
persisted access snapshot.

Server operations such as authentication, subscription sync and entitlement verification require connectivity.

Out of Scope

The current system does not include:

cloud synchronization of work data between devices;
production public online booking;
server-side client and appointment storage;
server push notifications through FCM/APNs;
collaborative multi-user workspace;
store-managed free trial in addition to the Aveli server trial.
Platform
Area	Technology
Mobile client	Flutter
State / DI	Riverpod
Local database	Drift / SQLite
Backend	NestJS
ORM	Prisma
Server database	PostgreSQL
Authentication	JWT access + rotating refresh
Billing	RevenueCat
Mobile platforms	Android / iOS
System Boundary
User
  │
  ▼
Flutter Application
  │
  ├── Local Workspace ──→ SQLite / Device Files
  │
  └── Account & Access ──→ HTTPS API ──→ NestJS / PostgreSQL
                                   │
                                   └── RevenueCat
Summary

Aveli is currently scoped as an offline-oriented personal workspace with a server-managed account, access and billing layer.

Operational work data remains on the user's device, while the backend controls identity and entitlement.