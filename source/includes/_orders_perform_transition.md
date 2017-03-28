## Переходы по статусам

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

### HTTP REQUESTS

`GET /orders/:id/transitions` - Доступные перемещения

`POST /orders/:id/transitions` - Совершить перемещение

### Доступные перемещения

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

> POST /orders/17/transitions

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

<aside class="notice">
Права на перемещения между статусами разделены между продавцом и покупателем.

Если проводить операцию не от того имени - произойдет ошибка 403. Если в параметре to_state указать недопустимый статус - произойдет ошибка 400
</aside>

