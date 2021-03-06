# Сервис «Счёт»

Название: Счёт

Назначение: Отвечает за баланс счёта пользователя. Отвечает за оплату заказа.

Запросы:
- Получить данны о счёте пользователя GET /api/v1/account/{userId}

Команды:
- Пополнить счёт PUT /api/v1/account/put/{userId}
- Снять деньги со счёта PUT /api/v1/account/withdraw/{userId}

События:
- Заказ успешно оплачен OrderPaid
- Оплата заказа отклонена OrderPayRejected

Зависимости:
- Событие регистрации пользователя UserRegistered
- Событие удаления профиля пользователя UserProfileRemoved
- Событие создание заказа OrderCreated

Вопросы:
- В дальнейшем возможно потребуется хранить историю о поплнении и снятии денег со счёта, нужно об этом подумать.
- Пополнение и снятие будет проходить чеерз сервис payment. Нужно ли его выделять или можно расположить в данном сервисе?
  Где в таком случае будут интеграции с различными платёжными системами?