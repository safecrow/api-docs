# Сделки через форму

API Safecrow позволяет отправлять запрос на создание сделки с указанием Email пользователя и базовых параметров сделки. Можно отправлять запрос с сервера партнера, либо через AJAX (для успешного AJAX запроса сайт должен соответствовать указанному в партнерском кабинете).

Пример работающей через AJAX формы есть на нашем гитхабе:

[Код](https://github.com/safecrow/integration-form)

[Демо](http://safecrow.github.io/integration-form)

### Описание запроса

`POST /orders/create_from_referal/:refcode`

refcode - Реферальный код партнера.


### Параметры в запросе

Параметры можно передать как сериализованную форму (x-www-form-urlencoded или multipart/form-data) или в JSON Payload

Ключ | Обязательно | Описание
--------- | ------- | -----------
role | да |	Роль пользователя в сделке. Может быть: supplier – продавец, consumer – покупатель
owner_email |	да | Email пользователя
contragent_email | да |	Email 2-й стороны 
title |	нет |	Заголовок сделки
order_description |	да |	Описание сделки
cost |	да |	Стоимость сделки
verify_days |	да |	Срок проверки после доставки
commission_payer | да |	Кто платит комиссию. Может быть: supplier – продавец, consumer – покупатель, 50/50 – 50% продавец, 50% покупатель
