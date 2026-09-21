# Sequence Diagram: Бронирование автомобиля в каршеринге

```mermaid
sequenceDiagram
    participant Client as Клиент
    participant App as Мобильное приложение
    participant Backend as Бэкенд-система
    participant Telematics as Телематический модуль

    Client->>App: Авторизация и выбор авто на карте
    App->>Backend: Запрос доступных автомобилей
    Backend-->>App: Список доступных авто
    Client->>App: Выбор авто и нажатие "Забронировать"
    App->>Backend: Создание брони (ID клиента, ID авто)
    Backend->>Backend: Проверка документов и баланса
    alt Бронирование успешно
        Backend->>Telematics: Команда на открытие авто
        Telematics-->>Backend: Подтверждение открытия
        Backend-->>App: Бронь подтверждена
        App-->>Client: Показать подтверждение и начать аренду
    else Бронирование отклонено
        Backend-->>App: Отказ в бронировании (причина)
        App-->>Client: Показать сообщение об ошибке
    end