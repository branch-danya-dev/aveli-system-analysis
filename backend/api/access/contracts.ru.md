# `GET /v1/access`

> Канонический клиентский эндпоинт состояния доступа.

## Запрос

Аутентификация:

```http
Authorization: Bearer <accessToken>
```

Тело запроса отсутствует.

Успешный ответ:

```text
200 OK
```

## Ответ — `AccessStatusView`

```json
{
  "hasAccess": true,
  "source": "subscription",
  "trialEndsAt": null,
  "accessEndsAt": "2026-09-28T00:00:00.000Z",
  "subscription": {
    "plan": "product-or-entitlement-id",
    "status": "active",
    "autoRenew": true,
    "expiresAt": "2026-09-28T00:00:00.000Z"
  },
  "reason": null,
  "requiresOnlineVerification": true,
  "verifiedAt": "2026-08-28T09:00:00.000Z",
  "nextVerificationRequiredAt": "2026-08-31T09:00:00.000Z"
}
```

## Поля контракта

| Поле | Тип | Значение |
|---|---|---|
| `hasAccess` | boolean | Итоговое решение о доступе к рабочему пространству. |
| `source` | перечисление | `lifetime`, `manual`, `subscription`, `trial`, `none`. |
| `trialEndsAt` | ISO 8601 UTC или null | Окончание регистрационного пробного периода, если применимо. |
| `accessEndsAt` | ISO 8601 UTC или null | Окончание ручного доступа, подписки или пробного периода; для бессрочного доступа — null. |
| `subscription` | объект или null | Нормализованное представление текущей подписки. |
| `subscription.plan` | строка | `productId ?? entitlementId`. |
| `subscription.status` | строка | Серверный `SubscriptionStatus`. |
| `subscription.autoRenew` | логическое значение или null | Состояние автопродления по данным провайдера. |
| `subscription.expiresAt` | ISO 8601 UTC или null | Окончание текущего периода подписки. |
| `reason` | строка или null | Причина отказа, если доступа нет. |
| `requiresOnlineVerification` | boolean | Требует ли источник доступа периодической онлайн-проверки. |
| `verifiedAt` | ISO 8601 UTC | Время, когда сервер рассчитал состояние. |
| `nextVerificationRequiredAt` | ISO 8601 UTC | Срок следующей проверки, переданный сервером. |

## Причины отказа

```text
trial_expired
subscription_expired
access_required
```

## Отказ как продуктовое состояние

Отсутствие права доступа — допустимое продуктовое состояние с HTTP 200.

Оно не представляется кодом 403.

## Отражение во Flutter

```text
lib/features/subscription/domain/entities/access_state.dart
AccessState.fromJson
```

Изменение структуры этого JSON ломает совместимость с текущим клиентом.
