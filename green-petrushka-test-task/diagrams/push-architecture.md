# Верхнеуровневая схема PUSH-уведомлений

```mermaid
flowchart TD
    A[Mobile App] -->|Получает push-token| B[FCM / APNs]
    A -->|Передает push-token| C[API Gateway]
    C --> D[User / Profile Service]
    D --> E[(Device Token DB)]

    F[Cart Service] -->|CART_ABANDONED| I[Message Broker]
    G[Order Service] -->|ORDER_CANCELLED / ORDER_STATUS_CHANGED| I
    H[Marketing Service] -->|MARKETING_CAMPAIGN_STARTED| I
    P[Payment Service] -->|PAYMENT_SUCCESS / PAYMENT_FAILED| I

    I --> J[Notification Service]

    J --> K[(Notification DB)]
    J --> L[(User Preferences DB)]
    J --> E
    J --> T[(Template DB)]

    J --> M[Push Queue]
    M --> N[Push Worker]

    N -->|Android| O[Firebase Cloud Messaging]
    N -->|iOS| Q[Apple Push Notification Service]

    O --> A
    Q --> A

    N --> R[(Delivery Logs)]
```

## Краткое описание схемы

1. Мобильное приложение получает `pushToken` от FCM/APNs.
2. Приложение передает токен через API Gateway в User/Profile Service.
3. User/Profile Service сохраняет токен устройства в Device Token DB.
4. Бизнес-сервисы публикуют события в Message Broker.
5. Notification Service получает события, проверяет настройки пользователя и формирует уведомление.
6. Push Worker отправляет уведомление через FCM/APNs.
7. Результат отправки сохраняется в Delivery Logs.
