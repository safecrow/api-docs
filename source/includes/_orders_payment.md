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

### Тестовые карты для шлюза "MandarinPay"

Параметр | Значение | Комментарий
---------- | --------------- | -------------------------------------------
Номер карты	| 4929509947106878 | Для имитации успешных транзакций без 3DSecure
Номер карты | 4692065455989192 | Для имитации успешных транзакций с 3DSecure
Номер карты | 4485913187374384 | Для имитации неуспешных транзакций без 3DSecure
Номер карты | 4556058936309366 | Для имитации неуспешных транзакций с 3DSecure
Имя держателя карты | CARD HOLDER
Срок действия | Любой валидный | (как минимум следующий месяц)
CVV | 123

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
return | Нет |	Ссылка, куда будет отправлен пользователь после совершения/отмены оплаты через «Мандарин»*
description |	Нет | (по умолчанию будет: "Оплата сделки № %order.id")	Будет указано в описании заказа в робокассе или «Мандарин»

### Возвращает

Ключ | Значение/Формат значения
--------- | -----------
consumer_pay |	JSON объект, окончательные стоимости оплаты
consumer_pay.gateway |	Сумма оплаты через шлюз робокассы (с добавленной комиссией)
consumer_pay.invoice |	Сумма оплаты при выставлении счета
consumer_pay.mandarin |	Сумма оплаты через шлюз «Мандарин» (с добавленной комиссией)
gateway_commission |	Комиссия шлюза робокассы в процентах
gateway_url |	Ссылка, на которую нужно перенаправить пользователя для оплаты через робокассу
mandarin_commission |	Комиссия шлюза «Мандарин» в процентах
mandarin_url |	Ссылка, на которую нужно перенаправить пользователя для оплаты через «Мандарин»

### Создание счета на оплату

<aside class="notice">
Сделка должна быть в статусе approved (если создана покупателем) или pending (если создана продавцом)
</aside>

> POST /orders/1858/payment

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
