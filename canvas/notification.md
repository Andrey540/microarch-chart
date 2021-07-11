# Сервис «Уведомления»

Название: Уведомления

Назначение: Отвечает отправку сообщений клиенту.

Запросы: -

Команды: -

События: -

Зависимости:
- Событие регистрации пользователя UserRegistered
- Событие изменения профиля пользователя UserProfileUpdated
- Событие удаления профиля пользователя UserProfileRemoved
- Событие завершение заказа OrderCompleted
- Событие отклонения заказа OrderRejected

Вопросы:
- Как будет осуществлятся дотсавка, sms, email? Этим будут заниматься отдельные сервисы или в рамках текущего?
- Возможно нужно будет хранить настройки отправки уведомлений пользователю.