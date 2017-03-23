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
	"accept_conditions" : true
}
```

> Response body:

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
email | нет, если указан phone | 
phone | нет, если указан email | 
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
> Response body:

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

> Response body:

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

> Response body:

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

