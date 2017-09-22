---
title: SafeCrow API Reference

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - users
  - orders
  - orders_payment
  - orders_perform_transition
  - orders_claim
  - orders_shipping
  - orders_billing_info
  - orders_set_billing_info
  - orders_change
  - orders_transitions
  - shipping_update_track_number
  - attachments
  - webhooks
  - errors
  - ajax

search: true
---

# Общее

## API URLS

`https://www.safecrow.ru/api/v1` - основной сервер

`http://dev.safecrow.ru/api/v1` - тестовый сервер

## Формат данных / формат запроса

Данные принимаются/отдаются в формате JSON.

Рекомендуется устаналвивать заголовки:


`Accept application/json`

`Content-Type application/json`


Все POST/PATCH запросы должны отправлять json в request payload (теле HTTP запроса)

## Авторизация приложения

> POST /sessions/register_user

```json
{
	"api_key" : "your_api_key",
	"secret" : "your_secret",
	"request_time" : "2017-02-10T08:56:34+0000",
}
```

<aside class="warning">
При запросах уровня приложения (созданию/входу от имени пользователя, управлению подписками) в теле запроса (POST/PATCH) или в параметрах URL(GET) обязательно должны быть переданы параметры авторизации.
</aside>

<aside class="notice">
api_secret - секретный ключ приложения (будет выдан вместе с api_key)
</aside>

Ключ | Обязательно | Описание
--------- | ------- | -----------
api_key |	да | Идентификатор приложения в формате UUID (Будет предоставлен отдельно)
request_time | да | Время запроса в формате ISO-8601
secret | да |	Расчитывается по формуле sha256(api_key+api_secret+request_time)

## Авторизация пользователя

<aside class="warning">
При запросах от имени пользователя, необходимо добавлять к параметрам URL или к телу запроса access_token
</aside>

[Получение access_token](#part-b8432b687e976e5f)
