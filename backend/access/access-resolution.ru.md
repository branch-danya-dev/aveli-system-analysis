# Бэкенд Aveli — определение доступа

> Проверенная серверная модель принятия решения о доступе к рабочему пространству.

## Канонический приоритет

Используется первый действующий источник в порядке:

```text
1. бессрочное право (`lifetime`)
2. временное ручное право (`manual_temporary`)
3. право по подписке `support`
4. право пробного периода (`trial`)
5. нет доступа (`none`)
```

## Загрузка данных

`AccessService` читает:

```text
все права пользователя из access_grants
+
подписки из subscriptions, где entitlement_id = 'support'
```

Чистая функция принятия решения:

```text
backend/src/access/access.decision.ts
```

## Условие доступа по подписке

Подписка даёт доступ, если:

```text
status ∈ {
  active,
  trialing,
  grace_period,
  past_due,
  cancelled
}
AND
current_period_end > now
```

Статусы без доступа:

```text
expired
revoked
```

Важное следствие:

```text
cancelled + будущий current_period_end
→ доступ сохраняется до конца оплаченного периода
```

## `AccessStatusView`

Канонический публичный контракт:

[`../api/access/`](../api/access/)

Он возвращает:

```text
hasAccess
source
trialEndsAt
accessEndsAt
subscription
reason
requiresOnlineVerification
verifiedAt
nextVerificationRequiredAt
```

## Необходимость онлайн-проверки

Подтверждённое поведение:

| Итоговый источник | `requiresOnlineVerification` |
|---|---:|
| `lifetime` | `false` |
| `manual` | `false` |
| `subscription` | `true` |
| `trial` | `true` |
| `none` | `true` |

## Причина отказа

Когда `hasAccess=false`:

| `reason` | Условие |
|---|---|
| `trial_expired` | пробный период закончился и подписка не даёт доступ |
| `subscription_expired` | запись о подписке существует, но больше не даёт доступ |
| `access_required` | остальные случаи |

## Срок следующей офлайн-проверки

Сервер всегда рассчитывает:

```text
nextVerificationRequiredAt =
  verifiedAt + SUBSCRIPTION_OFFLINE_GRACE_HOURS
```

Текущее значение по умолчанию:

```text
72 часа
```

Это параметр политики, управляемый окружением, а не неизменное продуктовое правило.

клиент на Flutter сохраняет проверенный снимок состояния в защищённом хранилище и может использовать возвращённый период при офлайн-работе.

При работе онлайн сервер остаётся источником окончательного решения.

## Пробный период

Регистрационный пробный период:

```text
30 дней по текущему продуктовому правилу
управляется сервером
один регистрационный пробный период на аккаунт
```

Его нельзя сбросить переустановкой приложения или удалением локальной базы данных.

## Отказ в доступе не владеет данными

```text
hasAccess = false
        ↓
рабочее пространство недоступно

НЕ

рабочее пространство удалено
```

## Связанные документы

- [`../api/access/`](../api/access/)
- [`../billing/`](../billing/)
- [`../../database/server/`](../../database/server/)
