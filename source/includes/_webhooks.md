# Уведомления(Webhooks)

## Создание

<aside class="notice">
Подписку на уведомление необходимо подтвердить. На url подписки отправляется GET запрос на подтверждение с параметрами.
</aside>

### HTTP REQUEST

`POST /subscriptions`

> POST /subscriptions

```json
{
	"api_key" : "your_api_key",
	"secret" : "your_secret",
	"request_time" : "2017-02-10T08:56:34+0000",
    "app_subscription" :
    {
      "url" : "https://example.com/update_order",
      "to_states" : ["done","paid","escalation"]
    }
}
```

> Response:

```http
HTTP/1.1 201 Created
```
```json
{
  "app_subscription" :
  {
    "to_states" : [
      "done",
      "paid",
      "escalation"
    ],
    "url" : "https://example.com/update_order",
    "subscription_id" : "44623c4b-8c82-40f6-a560-f91b7953e671",
    "confirmed": false
  }
}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
Аутентификация приложения | да | api_key, secret, request_time
app_subscription | да | JSON объект - данные подписки
app_subscription.subscription_id | нет | Идентификатор в формате uuid (если не передается - генерится системой)
app_subscription.to_states | да | Список статусов, при переходе на которые будет приходить уведомление (пример: [“paid”, “done”, “escalation“ ])
app_subscription.url | да | url, на который будут приходить уведомления

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
app_subscription | JSON объект - данные подписки
app_subscription.subscription_id | Идентификатор в формате uuid (если не передается - генерится системой)
app_subscription.url | url, на который будут приходить уведомления
app_subscription.to_states | Список статусов, при переходе на которые будет приходить уведомление (пример: [“paid”, “done”, escalation“” ])
app_subscription.confirmed | boolean - Подтверждена ли подписка (при создании всегда false)

## Автоматическое подтверждение

<aside class="notice">
Если подтверждение по тем или иным причинам не сработало - можно запросить подтверждение вручную.
</aside>

> GET https://example.com/update_order?action=confirmation&subscription_id=44623c4b-8c82-40f6-a560-f91b7953e671&publisher_token=3670fc4bbf06e22836bec1e9cef6648e17b4a869953584e01006620742a1a317

> Response:

```http
HTTP/1.1 200 OK
```
```json
{"subscriber_token" : "60414458779cc0f30a7c99595f3645f74e09edca2009f8990726e4e2aae2e910"}
```

Ключ | Значение/Формат значения
--------- | -----------
action | confirmation
subscription_id | Идентификатор подписки - uuid
publisher_token | Токен паблишера. считается по алгоритму: Sha256(‘sub’ + api_key + subscription_id)

Ответ должен содержать JSON объект:

Ключ | Значение/Формат значения
--------- | -----------
subscriber_token | Sha256(‘sub’ + api_secret + subscription_id)

## Ручное подтверждение

### HTTP REQUEST

`POST /subscriptions/:subscription_id/confirm`

> POST /subscriptions/:subscription_id/confirm

```json
{
  "api_key" : "your_api_key",
  "secret" : "your_secret",
  "request_time" : "2017-02-10T08:56:34+0000",
}
```

> Response

```http
HTTP/1.1 204 No Content
```

### Параметры в запросе
[Параметры авторизации приложения](#part-72c90712e1be8fe8)

### Возвращает

Статус 204

## Список

### HTTP REQUEST

`GET /subscriptions`

> GET /subscriptions?api_key=your_api_key&secret=your_secret&request_time=2017-02-10T14:13:46Z


> Response:

```http
HTTP/1.1 200 OK
```
```json
{
  "app_subscription" :
  {
    "to_states" : [
      "done",
      "paid",
      "escalation"
    ],
    "url" : "https://example.com/update_order",
    "subscription_id" : "44623c4b-8c82-40f6-a560-f91b7953e671",
    "confirmed": false
  }
}
```

### Параметры в запросе
[Параметры авторизации приложения](#part-72c90712e1be8fe8)

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
app_subscription | JSON объект - данные подписки
app_subscription.subscription_id | Идентификатор в формате uuid (если не передается - генерится системой)
app_subscription.url | url, на который будут приходить уведомления
app_subscription.to_states | Список статусов, при переходе на которые будет приходить уведомление (пример: [“paid”, “done”, escalation“” ])
app_subscription.confirmed | boolean - Подтверждена ли подписка (при создании всегда false)

## Удаление

### HTTP REQUEST

`DELETE /subscriptions`

```HTTP
DELETE /subscriptions?api_key=your_api_key&secret=your_secret&request_time=2017-02-10T14:13:46Z
```

> Response

```http
HTTP/1.1 204 No Content
```

### Параметры в запросе
[Параметры авторизации приложения](#part-72c90712e1be8fe8)

### Возвращает

Статус 204

## Получение данных

На каждое обновление статуса сделки, оповещаются(POST запрос) все подписки, где указан статус, куда эта сделка перемещается.

> POST https://example.com/update_order

```json
{
  "type" : "transition",
  "token" : "260386955d1cca81e7f6fa57dcc14bfbe58d3c7cd37f7e5fe26afe71ee5f9eab",
  "subscription_id" : "44623c4b-8c82-40f6-a560-f91b7953e671",
  "transition":
  {
    "order_id" : 1,
    "to_state" : "paid"
  }
}
```

> Response

```http
HTTP/1.1 204 No Content
```

### Передаваемые параметры

Ключ | Значение/Формат значения
--------- | -----------
type | Тип уведомления. Пока всегда transition
token | токен, подтверждающий паблишера.  Sha256(‘sub’ + api_key + api_secret+subscription_id)
subscription_id | Идентификатор подписки
transition | JSON объект
transition.order_id | ID сделки
transition.to_state |  Статус, на который была переведена сделка

### Требуемый ответ

Любой статус 20X
