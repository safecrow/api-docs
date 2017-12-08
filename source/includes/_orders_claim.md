## Претензия 

<aside class="notice">
Сделка должна быть в статусе paid (если тип shipping_missed) или shipping (для состальных типов)
</aside>

### HTTP REQUESTS

`POST /orders/:id/claim` - Создание

`GET /orders/:id/claim` - Просмотр. Возвращает Claim

### Создание

> POST /orders/1833/claim

```json
{
  "claim" : {
    "reason" : "wrong_package",
    "description" : "Вместо ботинок привезли дрель",
    "attachment" : {
      "file_name" : "screen2.png",
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
  "claim" : {
    "reason" : "wrong_package",
    "description" : "Вместо ботинок привезли дрель",
    "attachment" : {
      "link" : "/system/claims/attachments/000/000/042/original/screen2.png"
      "name" : "screen2.png"
    }
  }
}
```

### Параметры в запросе

Ключ | Обязательно | Описание
--------- | ------- | -----------
claim |	да | JSON объект
claim.reason |	да |	Причина претензии, может быть: <br/>shipping_missed – Товар не был отправлен (заводится в статусе сделки paid) <br/>wrong_package – Товар не соответствует описанию <br/>package_broken – Наличие повреждений <br/>missing_goods – Отсутствуют детали <br/>to_late – Не получил товар в определенный срок <br/>not_sended – Товар не был отправлен <br/>other – Другая причина
claim.description |	да |	Комментарий к претензии
claim.attachment |	[Вложенный файл](#part-728293b1f23ce809)

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
claim |	JSON объект
claim.id |	ИД претензии
claim.reason |	Причина претензии
claim.description |	Комментарий к претензии 
claim.attachment |	[Вложенный файл](#part-728293b1f23ce809)

