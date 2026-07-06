# Задание 2. Проектирование API

## Описание задачи

В мобильном приложении интернет-магазина «Петрушка Зеленая» создается экран выбора магазина партнера. На экране отображается список магазинов, логотипы, информация о ближайшей или быстрой доставке. При клике на плашку магазина должен выполняться переход на внешний ресурс партнера.

---

## 1. Пример REST API запроса

```http
GET /api/v1/partner-stores?lat=55.030204&lon=82.920430
Host: api.petrushka-green.ru
Authorization: Bearer {access_token}
Accept: application/json
```

### Назначение запроса

Запрос вызывается при переходе пользователя на экран «Выберите магазин».

### Query-параметры

| Параметр | Тип | Обязательный | Описание |
|---|---|---|---|
| `lat` | number | да | Широта пользователя. Нужна для расчета доступности доставки. |
| `lon` | number | да | Долгота пользователя. Нужна для расчета доступности доставки. |

---

## 2. Пример JSON-ответа

```json
{
  "screenTitle": "Выберите магазин",
  "stores": [
    {
      "id": "metro",
      "name": "METRO",
      "logoUrl": "https://cdn.petrushka-green.ru/logos/metro.png",
      "delivery": {
        "type": "nearest",
        "title": "Ближайшая доставка",
        "timeText": "сегодня 21:00-23:00"
      },
      "externalUrl": "https://online.metro-cc.ru",
      "isAvailable": true,
      "sortOrder": 1
    },
    {
      "id": "auchan",
      "name": "Ашан",
      "logoUrl": "https://cdn.petrushka-green.ru/logos/auchan.png",
      "delivery": {
        "type": "nearest",
        "title": "Ближайшая доставка",
        "timeText": "сегодня 18:00-20:00"
      },
      "externalUrl": "https://www.auchan.ru",
      "isAvailable": true,
      "sortOrder": 2
    },
    {
      "id": "vkusvill",
      "name": "ВкусВилл",
      "logoUrl": "https://cdn.petrushka-green.ru/logos/vkusvill.png",
      "delivery": {
        "type": "fast",
        "title": "Быстрая доставка",
        "timeText": "от 20 до 60 минут"
      },
      "externalUrl": "https://vkusvill.ru",
      "isAvailable": true,
      "sortOrder": 3
    },
    {
      "id": "victoria",
      "name": "ВИКТОРИЯ",
      "logoUrl": "https://cdn.petrushka-green.ru/logos/victoria.png",
      "delivery": {
        "type": "nearest",
        "title": "Ближайшая доставка",
        "timeText": "сегодня 17:00-19:00"
      },
      "externalUrl": "https://www.victoria-group.ru",
      "isAvailable": true,
      "sortOrder": 4
    }
  ]
}
```

---

## 3. Логика на стороне мобильного приложения

1. Пользователь открывает экран «Выберите магазин».
2. Мобильное приложение вызывает `GET /api/v1/partner-stores`.
3. Backend возвращает список доступных магазинов партнеров.
4. Приложение отображает магазины в порядке `sortOrder`.
5. При клике на плашку магазина приложение открывает значение из поля `externalUrl` во внешнем браузере или WebView.

---

## 4. Возможные ошибки

### 401 Unauthorized

Пользователь не авторизован или передан невалидный токен.

```json
{
  "errorCode": "UNAUTHORIZED",
  "message": "Пользователь не авторизован"
}
```

### 400 Bad Request

Не переданы обязательные координаты пользователя.

```json
{
  "errorCode": "INVALID_LOCATION",
  "message": "Не переданы координаты пользователя"
}
```

### 500 Internal Server Error

Непредвиденная ошибка на стороне backend.

```json
{
  "errorCode": "INTERNAL_ERROR",
  "message": "Внутренняя ошибка сервера"
}
```
