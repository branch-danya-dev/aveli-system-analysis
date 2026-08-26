# Aveli — Release Constraints

## Purpose

This document defines configuration and deployment constraints that must be satisfied before Aveli is shipped as a production build.

The goal is to prevent development settings, unsafe endpoints or incomplete billing configuration from reaching production.

---

## Release Principle

A production build must not rely on development behavior.

Conceptually:

```text
Development Configuration
        ↓
Release Validation
        ↓
Production-Safe Configuration

If required production conditions are not met, the build should fail validation rather than ship with a broken or unsafe setup.

API Configuration

The production application must use a valid HTTPS backend endpoint.

Allowed:

https://api.example.com

Not allowed:

http://...
localhost
127.0.0.1
10.0.2.2

Release builds must not depend on emulator or loopback addresses.

Standalone Mode

Aveli supports a development-only standalone mode.

AVELI_STANDALONE

This mode may be useful during local development or isolated testing.

It must not be enabled in production.

Release Build
     ↓
AVELI_STANDALONE = disabled

A release build with standalone mode enabled must fail validation.

RevenueCat Configuration

Production billing requires valid public SDK configuration for the target platform.

Relevant configuration includes:

REVENUECAT_ANDROID_API_KEY
REVENUECAT_IOS_API_KEY
REVENUECAT_ENTITLEMENT_ID

The entitlement currently expected by Aveli is:

support

Secret RevenueCat credentials must never be included in the client configuration.

Backend Secrets

The following must remain server-side:

database credentials;
JWT signing secrets;
RevenueCat secret credentials;
webhook authentication secrets;
other server integration secrets.

They must be provided through the backend runtime environment rather than mobile build configuration.

Build-Time Configuration

Aveli uses compile-time configuration values.

Examples:

Define	Purpose
AVELI_API_BASE	Backend API URL
REVENUECAT_ANDROID_API_KEY	Android RevenueCat public SDK key
REVENUECAT_IOS_API_KEY	iOS RevenueCat public SDK key
REVENUECAT_ENTITLEMENT_ID	Expected entitlement
AVELI_STANDALONE	Development-only standalone mode
AVELI_DEBUG_SEED	Debug/demo seed behavior

Production behavior must be derived from explicit release configuration rather than development defaults.

Debug Seed

Debug seed data may be used during development.

AVELI_DEBUG_SEED

Production builds must not silently initialize development or demo workspace data.

Debug seed behavior must remain limited to development/test scenarios.

Release Config Gate

Before shipping, Aveli validates release configuration.

The release gate checks conditions such as:

API uses HTTPS
        +
API is not emulator / loopback
        +
Standalone mode disabled
        +
Required billing config present

Failure of a mandatory condition should stop the release process.

CI Validation

Release configuration checks are part of automated validation.

Conceptually:

Source
  ↓
Analyze
  ↓
Tests
  ↓
Release Config Gate
  ↓
Build / Ship

This prevents production safety from depending only on manual developer review.

Android Release

The Android production build uses the application package:

com.aveli.aveli

Release signing requires the production upload keystore.

Sensitive signing configuration must remain outside normal public source control.

For example:

key.properties

should reference local or CI-provided signing credentials rather than embedding secrets in the repository.

iOS Release

The iOS build must use production:

bundle configuration;
signing identity;
provisioning configuration;
RevenueCat iOS public key;
HTTPS API endpoint.

Development-specific service endpoints must not be inherited by the production scheme.

Backend Release

The backend production environment must provide:

PostgreSQL connection configuration;
authentication secrets;
RevenueCat server credentials;
webhook security configuration;
production runtime environment variables.

The backend must not depend on credentials hardcoded into the repository.

Database Migrations

Backend database migrations must be applied deliberately during deployment.

Application Version
       ↓
Required Prisma Migrations
       ↓
Production Database
       ↓
Backend Start

The deployed backend schema must remain compatible with the application version being released.

Client Database Migrations

Local Drift / SQLite migrations must preserve existing workspace data.

A mobile update must not require the user to recreate:

clients;
appointments;
services;
payments;
visit history.

Local database compatibility is therefore part of release safety.

Billing Readiness

Before enabling production subscriptions, the following must be aligned:

Store Products
      ↓
RevenueCat Offerings
      ↓
support Entitlement
      ↓
Aveli Backend Mapping
      ↓
Client Access Model

A mismatch between these layers may cause a successful store purchase to fail to grant application access.

Environment Separation

Development and production configuration must remain clearly separated.

Development
├── emulator API
├── debug seed
└── optional standalone

Production
├── HTTPS API
├── real billing configuration
├── release signing
└── no development bypasses
Failure Strategy

Unsafe release configuration should fail early.

Preferred:

Invalid Configuration
       ↓
Build / CI Failure

Not:

Invalid Configuration
       ↓
Ship Application
       ↓
Runtime Failure for User
Release Checklist

Before shipping:

production API uses HTTPS;
loopback and emulator URLs are absent;
standalone mode is disabled;
debug seed is disabled;
correct RevenueCat public keys are configured;
entitlement mapping uses support;
backend secrets are server-side only;
production database migrations are ready;
mobile DB migrations are tested;
Android / iOS signing configuration is valid;
automated tests pass;
release configuration gate passes.
Summary

Aveli release safety follows one principle:

Development flexibility
        must not become
Production uncertainty

Production configuration is explicitly validated so that identity, billing, access and user data behavior remain predictable after release.