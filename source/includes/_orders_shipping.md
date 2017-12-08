## Доставка и возврат

### HTTP REQUESTS

`POST /orders/:id/shipping` - Доставка создание.

`GET /orders/:id/shipping` - Доставка просмотр. Возвращает tracking.

`POST /orders/:id/shipping_back` - Возврат создание.

`GET /orders/:id/shipping_back` - Возврат просмотр. Возвращает tracking.

### Создание доставки

<aside class="notice">
Сделка должна быть в статусе paid.
</aside>

> POST /orders/136/shipping

```json
{
  "tracking" : {
    "company" : "12333",
    "track_number" : "4323",
    "attachment" : {
      "file_name" : "screen.png",
      "content":"Base64",
      "content_type" : "image/png"
    }
  }
}
```

> Response

```http
HTTP/1.1 201 Created
```
```json
{
  "tracking" : {
    "id" : 42,
    "company" : "12333",
    "track_number" : "4323",
    "attachment" : {
      "link" : "/system/trackings/attachments/000/000/042/original/screen.png?1457423273",
      "name" : "screen.png"
    }
  }
}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
tracking |	да |	JSON объект
tracking.company | нет |	Компания доставки
tracking.track_number |	нет |	Номер доставки (строка)
tracking.attachment |	[Вложенный файл](#part-728293b1f23ce809)

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
tracking |	JSON объект
tracking.id |	ИД доставки
tracking.company |	Компания доставки
tracking.track_number |	Номер доставки
tracking.attachment |	[Вложенный файл](#part-728293b1f23ce809)
tracking.created_at |	Дата создания


### Создание возврата

<aside class="notice">
 Сделка должна быть в статусе await_returning.
</aside>

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
tracking |	да |	JSON объект
tracking.company | нет |	Компания доставки
tracking.track_number |	нет |	Номер доставки (строка)
tracking.attachment |	[Вложенный файл](#part-728293b1f23ce809)

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
tracking |	JSON объект
tracking.id |	ИД доставки
tracking.company |	Компания доставки
tracking.track_number |	Номер доставки
tracking.attachment |	[Вложенный файл](#part-728293b1f23ce809)
tracking.created_at |	Дата создания

