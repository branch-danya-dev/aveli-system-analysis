# Интеграция с RevenueCat

> Межкомпонентная интеграция подписки между мобильным Aveli, бэкендом Aveli, RevenueCat и магазинами приложений.

## Назначение

RevenueCat — слой абстракции Aveli над подписками магазинов приложений.

Он поддерживает:

- предложения для мобильного клиента;
- покупка;
- восстановление;
- идентификация пользователя;
- серверный поиск сведений о подписчике;
- сверка жизненного цикла по событиям вебхука.

RevenueCat **не** является окончательным источником решения о доступе к рабочему пространству Aveli.

## Навигация

- [`mobile-sdk.ru.md`](mobile-sdk.ru.md)
- [`backend-rest.ru.md`](backend-rest.ru.md)
- [`webhooks.ru.md`](webhooks.ru.md)
- [`identity-mapping.ru.md`](identity-mapping.ru.md)
- [`subscription-flow.puml`](subscription-flow.puml)

Поведение внутри компонентов:

- [`../../frontend/billing/`](../../frontend/billing/)
- [`../../backend/billing/`](../../backend/billing/)
