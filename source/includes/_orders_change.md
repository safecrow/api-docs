## Запрос на изменение

Через запрос на изменение сделки, происходят - продление защиты сделки, изменение условий (стоимости) и оформление возврата.

### HTTP REQUESTS

`POST /orders/:id/order_changes` - Создание. Возвращает order_change

`POST /orders/:order_id/order_changes/:id/confirm` - Подтверждение. Возвращает order_change

`POST /orders/:order_id/order_changes/:id/reject` - Отклонение. Возвращает order_change


### Просмотр запроса на изменение

Поле requested_change  при просмотре [сделки](#part-dec3052b853e0b29)

### Создание

<aside class="notice">
Статус сделки должен быть:<br/>

* shipping – при типе prolong_protection<br/>

* rejected – при типах change_conditions, purchase_return
</aside>

> POST /orders/136/order_changes

```json
{
  "order_change" : {
    "change_type" : "prolong_protection",
    "prolong_protection_to" : "2017-01-31T15:00:00+00:00"
  }
}
```

> Response

```http
HTTP/1.1 201 Created
```
```json
{
  "order_change" : {
    "id" : 65,
    "change_type" : "prolong_protection",
    "new_cost" : null,
    "created_at" : "2017-03-12T18:57:08.175+03:00",
    "prolong_protection_to" : "2017-04-30",
    "state" : "pending",
    "initiator_id" : 42,
    "confirmer_id" : 43,
    "initiator_type" : "supplier",
    "required_fields" : []
  }
}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
order_change | да |	JSON объект
order_change.change_type |	да	| Тип изменения, может быть: <br/>prolong_protection – Продление защиты сделки <br/>change_conditions – Изменение условий сделки <br/>purchase_return – Возврат товара
order_change.prolong_protection_to |	да, если тип – prolong_protection |	Дата, до которой защита сделки будет продлена, в формате iso-8601
order_change.new_cost |	да, если тип –change_conditions |	Новая стоимость сделки

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
order_change | JSON объект
order_change.id |	ИД изменения
order_change.change_type | Тип изменения
order_change.new_cost | Новая стоимость (заполнена только у Изменения с типом change_conditions, в других случаях всегда null)
order_change.prolong_protection_to | Дата, до которой продлится защита (iso-8086, только дата) (заполнена только у Изменения с типом prolong_protection, в других случаях всегда null)
order_change.created_at |	Дата создания (iso-8086) 
order_change.state |	Статус изменения (при создании всегда pending)
order_change.initiator_id |	ИД пользователя, создавшего изменение 
order_change.confirmer_id |	ИД пользователя, который должен утвердить изменение
order_change.initiator_type |	Роль создателя изменения (может быть supplier или consumer)
order_change.required_fields |	Поля, которые необходимо указать у сделки, чтобы применить order_change (сейчас это только consumer_billing_info, при возврате/изменении условий)

### Подтверждение изменения сделки


<aside class="notice">
При подтверждении запроса на изменение сделки, также меняются некоторые параметры сделки:<br/>

* prolong_protection – Меняет end_of_protection<br/>

* change_conditions – Устанавливает changed_cost у сделки и переводит сделку в статус received<br/>

* purchase_return – Переводит сделку в статус await_returning
</aside>

