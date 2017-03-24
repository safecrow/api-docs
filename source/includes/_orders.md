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

