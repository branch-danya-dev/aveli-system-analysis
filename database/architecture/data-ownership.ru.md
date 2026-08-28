# Aveli — Data Ownership

> Canonical ownership и source-of-truth boundaries для данных Aveli.

## Основная граница

```text
Professional Workspace
        ↓
   User Workspace

Identity & Access
        ↓
      Backend
```

Домены связаны access control, но не разделяют ownership.

> **Professional work data принадлежит изолированному user workspace. Identity и access state принадлежат backend-controlled account domain.**

Граница следует из `BR-020`–`BR-026` и `BR-043`–`BR-047`.

## Professional Workspace Domain

| Данные | Canonical Owner |
|---|---|
| Clients | Professional workspace |
| Services | Professional workspace |
| Appointments | Professional workspace |
| Payments | Professional workspace |
| Visit notes | Professional workspace |
| Visit photos / workspace media | Professional workspace |

Текущая product model не делает Aveli backend source of truth для этих записей.

Workspace data изолируются по пользователям. Данные одного workspace не должны становиться видимыми при активном workspace другого пользователя.

## Identity & Access Domain

| Данные | Canonical Owner |
|---|---|
| Account identity | Backend |
| Authenticated session authority | Backend |
| Trial state | Backend |
| Manual access grants | Backend |
| Lifetime access | Backend |
| Resolved subscription state | Backend |
| Effective workspace access decision | Backend |

Subscription evidence может приходить из mobile billing ecosystem, но effective access decision Aveli остается backend-controlled product responsibility.

## Access не владеет workspace data

```text
Valid Access → Workspace доступен
No Access     → Workspace недоступен
                ↓
              Persistent workspace data не изменяются
```

Access expiration не должен удалять или изменять persistent professional workspace information.

## Logout и Account Switching

Logout завершает active authenticated state, но не удаляет persistent workspace.

Account switching меняет active workspace context, но не переносит данные между workspace.

## External Data Authorities

Внешнее происхождение данных не делает внешнюю систему владельцем результирующей записи Aveli. Например, import device contact создает или дополняет Aveli client record, но исходный contact не становится owner этой записи.

## Features, меняющие границу

Модель нужно пересмотреть при появлении:
- multi-device workspace synchronization;
- cloud workspace backup;
- public booking с server-side professional data;
- shared или collaborative workspaces.

## Связанная документация

- [`../../business/requirements/business-rules.ru.md`](../../business/requirements/business-rules.ru.md)
- [`data-lifecycle.ru.md`](data-lifecycle.ru.md)
- [`../models/conceptual/domain-model.ru.md`](../models/conceptual/domain-model.ru.md)
