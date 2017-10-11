# Пользователи

## Регистрация

### HTTP REQUEST

`POST sessions/register_user`

> POST sessions/register_user

```json
{
	"api_key" : "your_api_key",
	"secret" : "your_secret",
	"request_time" : "2017-02-10T08:56:34+0000",
	"email" : "test@test.com",
	"accepts_conditions" : true
}
```

> Response:

```http
HTTP/1.1 200 OK
```
```json
{
	"user":
	{
		"id" : 1,
		"email" : "test@test.com",
		"phone" : null,
		"name" : null
	}
}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
Аутентификация приложения | да | api_key, secret, request_time
name | нет | Имя или ФИО
email | нет, если указан phone | обязательный параметр выплатах на банковские карты
phone | нет, если указан email | обязательный параметр выплатах на банковские карты
accepts_conditions | да | true, Сообщает, что пользователь ознакомлен с условиями использования сервиса

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
user | JSON объект
id | ID пользователя
name | Имя или ФИО
email | email
phone | телефон

## Аутентификация

### HTTP REQUEST

`POST /sessions/auth`

> POST /sessions/auth

```json
{
	"api_key" : "your_api_key",
	"secret" : "your_secret",
	"request_time" : "2017-02-10T08:56:34+0000",
	"user_id" : "1"
}
```
> Response:

```http
HTTP/1.1 200 OK
```
```json
{"access_token" : "535fd66a27df6c19002392b9a5e7c536f716033e7168cca4e782e7cb349c304f"}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
Аутентификация приложения | да | api_key, secret, request_time
user_id | да | ID пользователя

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
access_token | токен, с которым можно выполнять запросы от имени этого пользователя.

<aside class="notice">
При получении нового токена пользователя, предыдущий от имени этого пользователя полученный с помощью текущего приложения - удаляется
</aside>

## Информация о пользователе

Возвращает информацию о пользователе по email или номеру телефона - может быть полезно, если ID пользователя по тем или иным причинам потерялся.

### HTTP REQUEST

`POST /sessions/find_user`

> POST /sessions/find_user

```json
{
  "api_key" : "your_api_key",
  "secret" : "your_secret",
  "request_time" : "2017-02-10T08:56:34+0000",
  "email" : "test@test.com"
}
```

> Response:

```http
HTTP/1.1 200 OK
```
```json
{
  "user":
  {
    "id" : 1,
    "email" : "test@test.com",
    "phone" : null,
    "name" : null
  }
}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
Аутентификация приложения | да | api_key, secret, request_time
email | нет, если указан phone |
phone | нет, если указан email |

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
user | JSON объект
id | ID пользователя
name | Имя или ФИО
email | email
phone | телефон

## Изменение пользователя

Позволяет изменить email или телефон существующего пользователя

### HTTP REQUEST

`PATCH /sessions/update_user/:user_id`

> PATCH /sessions/update_user/:user_id

```json
{
  "api_key" : "your_api_key",
  "secret" : "your_secret",
  "request_time" : "2017-02-10T08:56:34+0000",
  "email" : "new_email@test.com",
  "phone" : "+71111111111"
}
```

> Response:

```http
HTTP/1.1 200 OK
```
```json
{
  "user":
  {
    "id" : 1,
    "email" : "new_email@test.com",
    "phone" : "+71111111111",
    "name" : null
  }
}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
Аутентификация приложения | да | api_key, secret, request_time
email | нет |
phone | нет |

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
user | JSON объект
id | ID пользователя
name | Имя или ФИО
email | email
phone | телефон

## Кредитные карты

<aside class="notice">
Возвращает только подтвержденные карты(статус 'success')
</aside>

### HTTP REQUEST

`GET  /me/credit_cards`

> GET  /me/credit_cards?access_token=4c6842c8e96173d5b993fae04b6268cb28c0576ce27a9c3dc64a6de8d28

> Response:

```http
HTTP/1.1 200 OK
```
```json
{
 "credit_cards" : [
    {
      "id" : 1,
      "card_number" : "469206XXXXXX9192",
      "status" : "success",
      "card_expiration_year" : 18,
      "card_expiration_month" : 11
    }
  ]
}
```

### Возвращает список карт пользователя

Ключ | Значение/Формат значения
--------- | -----------
credit_cards |  Массив из кредитный карт
id | ID карты
status | Статус привязки карты
card_number |  Замаскированный номер карты
card_expiration_month | Месяц
card_expiration_year | Год

`GET  /me/bind_card`

> GET  /me/bind_card?access_token=4c6842c8e96173d5b993fae04b6268cb28c0576ce27a9c3dc64a6de8d28

### Возвращает ссылку для привязки новой карты

Ключ | Значение/Формат значения
--------- | -----------
id | Идентификатор карты полученный от "Мандарин"
redirect_url | https://secure.mandarinpay.com/CardBindings/New + полученный id




## Банковские реквизиты

### HTTP REQUEST

`GET  /me/billing_infos`

> GET  /me/billing_infos?access_token=4c6842c8e96173d5b993fae04b6268cb28c0576ce27a9c3dc64a6de8d28

> Response:

```http
HTTP/1.1 200 OK
```
```json
{
  "billing_infos" : [
    {
      "id"  : 3,
      "billing_type"  : "bank_account",
      "holder_type" : "personal",
      "payment_params"  : {
        "name"  : "Брюс Уэйн",
        "bik" : "044525225",
        "account" : "30101810400000000225"
      },
      "title" : "30101810400000000225"
    }
  ]
}
```

### Возвращает
Массив с [ банковскими реквизитами ](#billing-info-array)

`POST  /me/billing_info` - создает банковские реквизиты для пользователя

> POST /me/billing_info

```json
{
  "billing_info" : [
  {
    "holder_type" : "personal",
    "billing_type" : "bank_account",
    "payment_params" : {
      "name" : "Брюс Уэйн",
      "bik" : "042202603",
      "account" : "30101810400000000225"
      "organization" : "Mememem",
      "inn" : "0238526872",
      "ogrn" : "5077746887312"
    },
    "title" : "30101810400000000225",
    }
  ]
}
```

> Response:

```http
HTTP/1.1 200 OK
```
```json

{
  "billing_info":
  {
    "id" : 64,
    "billing_type" : "bank_account",
    "holder_type" : "personal",
    "payment_params" : {
      "name" : "Брюс Уэйн",
      "bik" : "042202603",
      "account" : "30101810400000000225"
    },
    "title" : "30101810400000000225",
    "bank_name" : "АО \"АЛЬФА-БАНК\"
}

```
