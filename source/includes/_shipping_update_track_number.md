## Изменение track_number

### HTTP REQUEST

### Работает, когда товар в пути к покупателю

`PATCH /orders/:order_id/shipping/track_number` - изменяет track_number при условии, если действия происходят от имени продавца и сделка
имеет статус shipping.

> PATCH /orders/:order_id/shipping/track_number

```json
{
  "tracking" : {
    "track_number" : "45"
  }
}
```

> Response

```json
{
  "status" : "updated"
}
```

### Работает, когда покупатель отказался по тем или иным причнинам от товара и товар в пути к продавцу

`PATCH /orders/:order_id/shipping_back/track_number` - изменяет track_number при условии, если действия происходят от имени покупателя и сделка
имеет статус shipping_back.

> PATCH /orders/:order_id/shipping_back/track_number

```json
{
  "tracking" : {
    "track_number" : "45"
  }
}
```

> Response

```json
{
  "status" : "updated"
}
```
