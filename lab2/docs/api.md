API Documentation — Lab 2 (Node-RED)

В данной лабораторной работе реализованы три GET-эндпоинта средствами Node-RED (ноды `http in` + `function` + `http response`). Все эндпоинты доступны локально по базовому адресу:

`http://localhost:1880`

---

## 1. GET /api/text

Возвращает простой текстовый ответ.

**Параметры:**
Нет.

**Пример запроса:**
GET [http://localhost:1880/api/text](http://localhost:1880/api/text)

**Пример успешного ответа:**
Статус: 200 OK
Content-Type: text/plain

Тело ответа:
Привет! Это простой текстовый ответ от Node-RED.

**Возможные ошибки:**
Отсутствуют.

---

## 2. GET /api/info

Возвращает JSON-объект с двумя полями.

**Параметры:**
Нет.

**Пример запроса:**
GET [http://localhost:1880/api/info](http://localhost:1880/api/info)

**Пример успешного ответа:**
Статус: 200 OK
Content-Type: application/json

Тело ответа:

```json
{
  "application": "Node-RED",
  "labNumber": 2
}
```

**Возможные ошибки:**  
Отсутствуют.

---



## **3. GET /api/items/:id**

Возвращает информацию о товаре по его идентификатору. Использует path-параметр `:id` и query-параметр `limit`.

**Параметры:**

1. `id` (path, обязательный) — числовой идентификатор товара. Доступные значения: 1, 2, 3.
2. `limit` (query, опциональный) — ограничение количества выводимых записей. По умолчанию равно 10.

**Пример успешного запроса:**  
GET [http://localhost:1880/api/items/1](http://localhost:1880/api/items/1)

**Пример успешного ответа:**  
Статус: 200 OK  
Content-Type: application/json

Тело ответа:

json

```
{
  "id": "1",
  "item": {
    "name": "Яблоко",
    "price": 100
  },
  "limit": 10
}
```

**Пример запроса с query-параметром:**  
GET [http://localhost:1880/api/items/2?limit=5](http://localhost:1880/api/items/2?limit=5)

**Пример успешного ответа:**  
Статус: 200 OK

Тело ответа:

json

```
{
  "id": "2",
  "item": {
    "name": "Банан",
    "price": 50
  },
  "limit": "5"
}
```

**Пример ошибки 400 (Bad Request):**  
Возникает, когда `id` не является числом или отсутствует.  
Запрос: GET [http://localhost:1880/api/items/abc](http://localhost:1880/api/items/abc)

Статус: 400 Bad Request

Тело ответа:

json

```
{
  "error": "Bad Request: ID должен быть числом"
}
```

**Пример ошибки 404 (Not Found):**  
Возникает, когда товара с указанным `id` нет в «базе данных».  
Запрос: GET [http://localhost:1880/api/items/999](http://localhost:1880/api/items/999)

Статус: 404 Not Found

Тело ответа:

json

```
{
  "error": "Not Found: Товар с id 999 не найден"
}
```

---



## **Сводная информация по эндпоинтам**

1. **GET /api/text**
  - Успешный статус: 200 OK
  - Статусы ошибок: нет
  - Описание: Простой текст
2. **GET /api/info**
  - Успешный статус: 200 OK
  - Статусы ошибок: нет
  - Описание: JSON с двумя полями
3. **GET /api/items/:id**
  - Успешный статус: 200 OK
  - Статусы ошибок: 400 Bad Request, 404 Not Found
  - Описание: Информация о товаре с path/query параметрами

---



## **Логика работы эндпоинта /api/items/:id**

1. Извлечение параметров:
  - `msg.req.params.id` — path-параметр (обязательный).
  - `msg.req.query.limit` — query-параметр (опциональный, по умолчанию 10).
2. Валидация:
  - Если `id` отсутствует или не является числом (`isNaN(id)`), возвращается статус 400 Bad Request.
3. Поиск в «базе данных» (имитация):
  - Если товар с таким `id` не найден, возвращается статус 404 Not Found.
4. Успешный ответ:
  - Формируется JSON-объект с полями `id`, `item`, `limit` и статусом 200 OK.

---



## **Как проверить через браузер**

Открой любой из URL в браузере:

- [http://localhost:1880/api/text](http://localhost:1880/api/text)
- [http://localhost:1880/api/info](http://localhost:1880/api/info)
- [http://localhost:1880/api/items/1](http://localhost:1880/api/items/1)
- [http://localhost:1880/api/items/2?limit=5](http://localhost:1880/api/items/2?limit=5)
- [http://localhost:1880/api/items/abc](http://localhost:1880/api/items/abc) (ошибка 400)
- [http://localhost:1880/api/items/999](http://localhost:1880/api/items/999) (ошибка 404)



## **Как проверить через curl**

bash

```
# Успешный запрос
curl -i http://localhost:1880/api/items/1

# Ошибка 400
curl -i http://localhost:1880/api/items/abc

# Ошибка 404
curl -i http://localhost:1880/api/items/999
```



## **Как проверить через Postman**

1. Создай новый запрос GET.
2. Вставь URL, например [http://localhost:1880/api/items/1](http://localhost:1880/api/items/1).
3. Нажми Send.
4. Посмотри на Status (200/400/404) и тело ответа.

