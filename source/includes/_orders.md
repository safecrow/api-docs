# Сделки

## Создание

### HTTP REQUEST

`POST /orders`

> POST /orders

```json
{
  "order" : {
    "order_description" : "Сделка на поставку",
    "verify_days" : "3",
    "commission_payer" : "consumer",
    "cost" : "23",
    "supplier_id" : "21"
  }
}
```

> Response

```http
HTTP/1.1 201 Created
```
```json
{
  "order" : {
    "id" : 17,
    "state" : "pending",
    "title" : null,
    "order_description" : "Сапоги",
    "supply_description" : null,
    "cost" : 23.0,
    "supply_cost" : null,
    "supply_days" : null,
    "verify_days" : 3,
    "commission_payer" : "consumer",
    "commission" : 6.0,
    "consumer_pay" : 24.37,
    "supplier_receive" : 23.0,
    "pay_back" : 0,
    "owner_id" : 21,
    "transitions" : [],
    "created_at" : "2017-03-03T13:34:42.250+03:00",
    "end_of_protection" : null,
    "escalation_date" : null,
    "client_data" : null,
    "allowed_changes" : [],
    "requested_change" : null,
    "changed_cost" : null,
    "role" : "supplier",
    "billing_info" : null,
    "commission_description" : "4%",
    "attachments" : [],
    "consumer_id" : null,
    "supplier_id" : 21,
    "consumer" : null,
    "supplier" : {
      "id" : 21,
      "email" : "test7@test.com",
      "phone" : null,
      "name" : null
    }
  }
}
```


### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
order | да | Json объект
order.order_description | да | Описание сделки
order.supplier_id | да | Продавец  (Если покупатель не заполняется, обязательно id текущего пользователя)
order.cost | да | Стоимость сделки(минимальная стоимость 20р)
order.verify_days | да | Срок проверки, по умолчанию будет установлено 3 недели срок защиты
order.commission_payer | да | Кто оплачивает комиссию.  Возможны варианты: supplier, consumer, 50/50
order.title | нет | Заголовок сделки
order.consumer_id | нет | Покупатель
order.supply_cost | нет | Стоимость доставки *Поле является информативным - значение на расчет стоимости сделки и комиссии влиять не будет
order.supply_description | нет | Комментарий к доставке
order.supply_days | нет | Срок доставки
order.client_data | нет | Любые данные в формате json
order.attachments |	нет |	Массив из [вложенных файлов](#part-728293b1f23ce809)

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
order | JSON объект
order.id | ИД сделки
order.state | Текущий статус (при создании всегда pending)
order.title | Заголовок сделки
order.order_description | Описание заказа
order.supply_description | Комментарий к доставке
order.cost | Стоимость сделки
order.changed_cost | Измененная стоимость сделки (отображается, если есть принятый запрос на изменение, с типом change_conditions)
order.supply_cost | Стоимость доставки
order.supply_days | Срок доставки
order.verify_days | Срок проверки
order.commission_payer |  Кто оплачивает комиссию
order.commission | Комиссия в рублях
order.commission_description |	Текстовое описание комиссии (например "4%")
order.consumer_pay | Сумма, которую платит покупатель
order.supplier_receive | Сумма, которую получит продавец
order.pay_back | Сумма, которая вернется покупателю (Проставляется в случае смены условий сделки. Если изменилась стоимость сделки - разница между суммами, если произошел возврат - стоимость сделки)
order.owner_id | Создатель сделки
order.supplier_id | ИД продавца
order.supplier | Продавец (user)
order.consumer_id | ИД покупателя
order.consumer | Покупатель (user)
order.client_data | Данные в формате json, введенные при создании сделки
order.end_of_protection | Дата окончания защиты сделки. Считается автоматически из (Срок доставки + срок проверки). Обнуляется при подаче жалобы
order.escalation_date | Дата автоматической эскалации конфликта (проставляется автоматически, при подаче жалобы - 7 дней после подачи жалобы)
order.attachments |	Массив из [вложенных файлов](#part-728293b1f23ce809)
order.manager_comment | Комментарий менеджера (не доступно для редактирования - вводится менеджером safecrow)
order.role | Роль текущего пользователя в сделке (supplier|consumer)
billing_info |	Платежные данные текущего пользователя для этой сделки
order.allowed_changes | JSON массив - список доступных типов изменений (order_change - подробнее в п. 6)
order.allowed_changes[].change_type | Тип изменения
order.allowed_changes[].required_fields | Список полей сделки, которые необходимо заполнить для создания перемещения (на данный момент бывает только consumer_billing_info - для изменения условий сделки и запроса на возврат)
order.allowed_changes[].locale | JSON объект - локаль
order.allowed_changes[].locale.change_type | Русское обозначение типа изменения
order.requested_change | JSON объект - запрос на изменение сделки (пока существует не подтвержденный (или отклоненный) запрос на изменение - новые создать нельзя)
order.transitions | JSON массив - информация о возможных изменениях статуса (подробнее п. 5)
order.transitions[n].to_state | Статус, на который можно совершить перемещение
order.transitions[n].can_perform | boolean, указывает все ли условия соблюдены для перехода на следующий статус
order.transitions[n].required_fields | Массив строк - список полей/вложенных объектов сделки, которые нужно указать для перемещения (если can_perform=true - всегда пустой)
order.transitions[n].locale | JSON объект - русскоязычная информация
order.transitions[n].locale.to_state | Обозначение следующего статуса
order.transitions[n].locale.transition | Обозначение действия перемещения
order.locale | Локаль
order.locale.state | Русскоязычоное обозначение статуса
order.locale.display_state | Обозначение статуса с учетом текущего запроса изменения (может быть например  - Продавец/покупатель зарпосил продление сделки)

## Расчет комиссии

Позволяет предварительно расчитать комиссию и стоимость для различных вариантов оплаты

### HTTP REQUEST

`POST /orders/calc_commission`

> POST /orders/calc_commission

```json
{
  "cost" : 10000000,
  "commission_payer" : "50/50"
}
```

> Response

```http
HTTP/1.1 200 OK
```
```json
{
  "cost" : 10000000.0,
  "commission" : 100000.0,
  "commission_description" : "1%",
  "mandarin_commission" : 2.4,
  "gateway_commission" : 2.9,
  "supplier_receive" : 9950000.0,
  "consumer_pay" : {
    "invoice" : 10050000.0,
    "gateway" : 10341450.0,
    "mandarin" : 10211050.0
  }
}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
cost | да | Стоимость сделки
commission_payer | да |	Кто будет оплачивать комиссию (consumer|supplier|50/50)

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
cost |	Стоимость сделки (будет равна указанному значению cost из запроса)
commission | Комиссия SafeCrow
commission_description |	Текстовое описание комиссии (например: 4%)
supplier_receive |	Сумма, которую получит продавец
mandarin_commission |	Комиссия шлюза мандарина в %
gateway_commission |	Комиссия шлюза робокассы в %
consumer_pay | Стоимость оплаты в зависимости от варианта, JSON объект
consumer_pay.invoice |	Стоимость оплаты через банковский счет
consumer_pay.gateway |	Стоимость оплаты через шлюз робокассы (с учетом комиссии шлюза)
consumer_pay.mandarin |	Стоимость оплаты через шлюз мандарин (с учетом комиссии шлюза)

## Изменение сделки

Изменение отдельных параметров заказа.

<aside class="notice">
Изменить можно любое редактируемое (из тех, которые задаются при создании) поле, пока сделка не подтверждена покупателем.
</aside>

### HTTP REQUEST

`PATCH /orders/:id`

> PATCH /orders/9

```json
{
  "order" : {
    "consumer_id" : "14"
  }
}
````

> Response

```http
HTTP/1.1 200 OK
```
```json
{
  "order" : {
    "id" : 9,
    "state" : "pending",
    "order_description" : "Сапоги",
    "supply_description" : null,
    "cost" : 400.0,
    "supply_cost" : null,
    "supply_days" : null,
    "verify_days" : 4,
    "commission_payer" : "consumer",
    "commission" : 6.0,
    "owner_id" : 13,
    "supplier_billing_info" : null,
    "consumer_id" : 14,
    "consumer_billing_info" : null,
    "consumer" : {
      "id" : 14,
      "email" : "test4@test.com",
      "phone" : "90813323347",
      "name" : null
    },
    "supplier_id" : 13,
    "supplier" : {
      "id" : 13,
      "email" : "test2@test.com",
      "phone" : "90813323346",
      "name" : null
    }
  }
}
```

### Возвращает

[Объект сделки](#part-dec3052b853e0b29)

## Список

Выводит список сделок, в которых участвует текущий пользователь. Сделки отсортированы по времени создания в обратном порядке (последние сначала)

### HTTP REQUEST

`GET /orders`

> GET /orders

> Response

```http
HTTP/1.1 200 OK
```
```json
{
  "orders" : [
     {
       "id" : 17,
       "state" : "pending",
       "order_description" : "Сапоги",
       "supply_description" : null,
       "cost" : 23.0,
       "supply_cost" : null,
       "supply_days" : null,
       "verify_days" : 3,
       "commission_payer" : "consumer",
       "commission" : 6.0,
       "owner_id" : 21,
       "consumer_id" : null,
       "supplier_id" : 21,
       "consumer" : null,
       "supplier" : {
         "id" : 21,
         "email" : "test7@test.com",
         "phone" : null,
         "name" : null
       }
     }
  ]
  "meta" : {
    "page" : 1,
    "total" : 3,
    "total_pages" : 1,
    "links" : {
      "self" : "http://localhost:3000/api/v1/orders?page=1",
      "next_page" : null,
      "previous_page" : null,
      "first_page" : null,
      "last_page" : null
    }
  }
}
```

### Параметры в запросе

`page` - номер страницы

`per` - количество сделок на одной странице (по умолчанию - 10)

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
orders | Массив из сделок [order](#part-dec3052b853e0b29)
meta | JSON объект - метаданные коллекции
meta.page | Номер текущей страницы
meta.total | Общее количество сделок, в которых участвует текущий пользователь
meta.total_pages | Общее количество страниц
meta.links | JSON объект
meta.links.self | Текущая страница
meta.links.next_page | Следующая страница
meta.links.previous_page | Предыдущая страница
meta.links.first_page | Первая страница
meta.links.last_page | Последняя страница

## Отображение

### HTTP REQUEST

`GET /orders/:id`

> GET /orders/9

> Response

```http
HTTP/1.1 200 OK
```
```json
{
  "order" : {
    "id" : 9,
    "state" : "pending",
    "order_description" : "Сапоги",
    "supply_description" : null,
    "cost" : 400.0,
    "supply_cost" : null,
    "supply_days" : null,
    "verify_days" : 4,
    "commission_payer" : "consumer",
    "commission" : 6.0,
    "owner_id" : 13,
    "supplier_billing_info" : null,
    "consumer_id" : 14,
    "consumer_billing_info" : null,
    "consumer" : {
      "id" : 14,
      "email" : "test4@test.com",
      "phone" : "90813323347",
      "name" : null
    },
    "supplier_id" : 13,
    "supplier" : {
      "id" : 13,
      "email" : "test2@test.com",
      "phone" : "90813323346",
      "name" : null
    }
  }
}
```

### Возвращает

Возвращает [сделку](#part-dec3052b853e0b29) с переданным id.

## Оплата

<aside class="notice">
После возвраты из робокассы на redirect_url будет дополнительно задан параметр URL - payment=success либо payment=fail в зависимости от успешности оплаты.
https://example.com/callback?payment=success – оплата прошла успешно
https://example.com/callback?payment=fail – оплата была отменена
</aside>

<aside class="notice">
После возвраты из mandarin на return будет дополнительно задан параметр URL - status=success либо status=fail в зависимости от успешности оплаты.
https://example.com/callback?status=success – оплата прошла успешно
https://example.com/callback?status=fail – оплата была отменена
</aside>

<aside class="warning">
Для оплаты через mandarin у пользователя, от имени которого делается запрос, обзятально должно быть заполнено поле email. Иначе в поле mandarin_url вернется значение null.
</aside>

### HTTP REQUESTS

`GET /orders/:id/payment_info` - Информация о стоимости и оплате

`POST /orders/:id/payment` - Создание счета на оплату

`GET /orders/:id/payment` - Просмотр счета на оплату. Возвращает Payment.

`GET /orders/:id/download_invoice` - Ссылка на скачивание квитанции. Возвращает cформированную квитанцию для оплаты в формате .docx

### Информация о стоимости и оплате

> GET /orders/1858/payment_info?redirect_to=http%3A%2F%2Fruby.org

> Response

```http
HTTP/1.1 200 OK
```
```json
{
  "consumer_pay" : {
    "invoice" : 23.92,
    "gateway" : 24.61,
    "mandarin" : 24.10
  },
  "gateway_commission" : 2.9,
  "gateway_url" : "https://merchant.roboxchange.com/Index.aspx?MrchLogin=safecrow-test&OutSum=24.61&InvId=1858135&SignatureValue=c9289a9999e9c1584dcccbd08b5617ca&shporder_id=1858&shpredirect_to=http://ruby.org",
  "mandarin_commission" : 2.4,
  "mandarin_url" : "https://secure.mandarinpay.com/Pay/Select?transaction=080225790640482092ba95ab2671432c"
}
```

В случае оплаты через выставление счета, также будет актуальна стоимость из поля сделки consumer_pay

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
redirect_to |	Нет |	Ссылка, куда будет отправлен пользователь после совершения/отмены оплаты*
return | Нет |	Ссылка, куда будет отправлен пользователь после совершения/отмены оплаты через мандарин*
description |	Нет | (по умолчанию будет: "Оплата сделки № %order.id")	Будет указано в описании заказа в робокассе или мандарин

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
consumer_pay |	JSON объект, окончательные стоимости оплаты
consumer_pay.gateway |	Сумма оплаты через шлюз робокассы (с добавленной комиссией)
consumer_pay.invoice |	Сумма оплаты при выставлении счета
consumer_pay.mandarin |	Сумма оплаты через шлюз мандарин (с добавленной комиссией)
gateway_commission |	Комиссия шлюза робокассы в процентах
gateway_url |	Ссылка, на которую нужно перенаправить пользователя для оплаты через робокассу
mandarin_commission |	Комиссия шлюза мандарин в процентах
mandarin_url |	Ссылка, на которую нужно перенаправить пользователя для оплаты через мандарин

### Создание счета на оплату

<aside class="notice">
Сделка должна быть в статусе approved (если создана покупателем) или pending (если создана продавцом) 
</aside>

> POST /api/v1/orders/1858/payment

```json
{
  "payment" : {
     "name" : "Иванов Иван Иванович",
     "payment_method" : "invoice",
     "payment_info" : {"level" : "over 9000"}
  }
}
```

> Response

```http
HTTP/1.1 201 Created
```
```json
{
  "payment" : {
    "id" : 332,
    "name" : "Иванов Иван Иванович",
    "payment_method" : "invoice",
    "info" : {"level" : "over 9000"}
  }
}
```
### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
payment |	Да |JSON объект
name | Да |	ФИО Плательщика
payment_method | Да |	Всегда указывать invoice
info | Нет |	Дополнительная информация - произвольный JSON объект (данные партнера)

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
payment | JSON объект
payment.id |	ИД платежа
payment.name |	ФИО Плательщика
payment.payment_method |	Всегда invoice
payment.info |	JSON объект

## Переходы по статусам

### HTTP REQUESTS

`GET /orders/:id/transitions` - Доступные перемещения

`POST /orders/:id/transitions` - Совершить перемещение

### Доступные перемещения

> GET /orders/17/transitions

> Response

```http
HTTP/1.1 200 OK
```
```json
{
  "transitions" : [
     {
       "to_state" : "payment_verification",
       "can_perform" : false,
       "required_fields" : [
         "payment"
       ],
       "locale" : {
         "to_state" : "Оплачена, ожидается проверка менеджером",
         "transition" : "Подтвердить и перейти к оплате"
       }
     }
  ]
}
```

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
transitions | JSON Массив (информация о перемещении)
transitions[n].to_state | Статус, на который можно совершить перемещение
transitions[n].can_perform | boolean, указывает все ли условия соблюдены для перехода на следующий статус
transitions[n]. required_fields | Массив строк - список полей/вложенных объектов сделки, которые нужно указать для перемещения (если can_perform=true - всегда пустой)
transitions[n].locale | JSON объект - русскоязычная информация
transitions[n].locale.to_state | Русскоязычное обозначение следующего статуса
transitions[n].locale.transition | Русскоязычное обозначение действия перемещения

### Совершить перемещение

<aside class="notice">
Права на перемещения между статусами разделены между продавцом и покупателем.

Если проводить операцию не от того имени - произойдет ошибка 403. Если в параметре to_state указать недопустимый статус - произойдет ошибка 400
</aside>

> POST /api/v1/orders/17/transitions

```json
{
  "transition" : {
    "to_state" : "payment_verification"
  }
}
```

> Response

```http
HTTP/1.1 204 No content
```
