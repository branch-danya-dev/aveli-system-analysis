# Dart http

> Thin HTTP transport Aveli auth/access data sources.

## Verified Version

`http 1.6.0`

## Role

Client явно строит requests к backend account/access APIs. Dio dependency отсутствует, centralized HTTP interceptor middleware нет.

Bearer headers добавляются data sources; access requests могут выполнить single 401 refresh/retry path.

## Replaceability

**High.** Wrapper thin и хорошо изолирован относительно state/domain.
