## Цепочка перемещений

### Роли пользователей, совершающих перемещения по статусам.

### Продавец

**pending => approved**

Условия: 

* Заполнены платежные реквизиты продавца

**paid => shipping**

Условия:

* Заполнена информация о доставке

* Заполнены платежные реквизиты продавца

**returning => returned**

### Покупатель

**pending => payment_verification**

Условия:

* Должна пройти оплата или сформирована квитанция

**shipping => received**

**shipping => rejected**

Условия:

* Должна быть подана жалоба

**rejected => received**

### Автоматические переходы:

**payment_verification => paid**

Условие: Оплата через gateway

**rejected => escalation**

Условие: Через 7 дней, после подачи жалобы.

**shipping => received**

Условие: Когда заканчивается срок защиты сделки

**rejected => received**

Условие: Принят запрос на изменение условий сделки

**rejected => await_returning**

Условие: Принят запрос на возврат товара

**received => payout_initiated**

**returned => payout_initiated**

**payout_initiated => done** (При выплате на карты)

### Переходы, совершаемые менеджером SafeCrow

**rejected => escalation**

**payment_verification => paid** (при оплате квитанцией)

**payout_initiated => done** (При выплате по расчетному счету)

