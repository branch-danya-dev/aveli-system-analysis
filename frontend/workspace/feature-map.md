# Workspace Feature Map

## Clients

```text
Presentation
→ ClientsScreen / ClientDetailsScreen / editor / contact import
State
→ clients_providers.dart
Domain
→ ClientRepository + create/update/delete/archive/import use cases
Data
→ client_repository_impl.dart / client_directory_repository_impl.dart
Persistence
→ Drift clients
```

## Services

```text
Presentation → ServicesScreen / editor
State        → services_providers.dart
Domain       → ServiceRepository / SaveService / DeleteService
Data         → service_repository_impl.dart
Persistence  → Drift services
```

## Appointments / Visits

```text
Presentation → create/details/complete/cancel flows
State        → appointments_providers.dart
Domain       → appointment + visit use cases
Data         → appointment_repository_impl / visit_repository_impl
Persistence  → appointments + visit_notes + visit_photos
Cross        → reminders + payments + clients + services
```

## Today / Calendar

Reactive projections are backed by Drift table updates.

```text
TodayScreen / CalendarScreen / DayBoard
→ providers
→ repositories
→ ScheduleDayProjection / Drift joins
```

## Payments / Debts

```text
OutstandingPaymentsScreen
PeriodFinanceScreen
AcceptPaymentSheet
→ payments providers/use cases
→ payment repositories
→ payments + appointments.payment_status
```

## Settings / Profile

Profile/settings are local application data, not a mirror of backend `users`.

Persistence uses `app_settings`.

Included client behaviors include appearance, local profile, public-listing flags, import/export, and currency conversion.

## Schedule

`ScheduleConfig` provides working start/end minutes used by appointment creation, slot validation and day-board layout.
