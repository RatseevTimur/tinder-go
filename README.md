# tinder-go
tinder clone (GO, React, TS, Kafka, REST/gRPC, Redis, PostgreSQL, MongoDB, Docker, Kubernetes)


🚀 Архитектура TINDER-style APP (на миллиарды пользователей)
Технологии:

Backend: Go

Frontend: TypeScript + React

Брокер сообщений: Kafka

Кеш и pub/sub: Redis

База данных: PostgreSQL (основная), MongoDB (для неструктурированных данных)

Auth: JWT или session-based, Redis для хранения активных сессий

Контейнеризация: Docker + Kubernetes (опционально)

API: gRPC + REST (в зависимости от сервиса)

Логи: Loki + Promtail + Grafana

Мониторинг: Prometheus

🧩 Микросервисы

Сервис	Описание
AuthService	Регистрация, логин, refresh token, хранение сессий
UserService	Профили, фотографии, предпочтения
MatchService	Алгоритм матчей, свайпы, обработка лайков/дизлайков
ChatService	Вебсокеты или Kafka-based чаты
NotificationService	Email, push уведомления
RecommendationService	Машинное обучение или эвристика для подбора анкет
AdminService	Просмотр логов, управление пользователями
Gateway API	BFF (backend-for-frontend), агрегирует все сервисы
🔁 Взаимодействие через Kafka
Примеры событий:

user.created, user.updated

match.created, match.canceled

chat.message.sent, chat.message.read

notification.triggered

Каждый сервис слушает и эмитит ивенты. Например:

```go
// match-service receives "swipe.right"
kafkaConsumer.On("swipe.right", func(data SwipeEvent) {
   // Проверка match
   // Если match -> отправить match.created
})
```
🧠 
MVP-функциональность
Frontend MVP: регистрация → вход → просмотр анкет → свайп → отображение матчей

Backend MVP:

AuthService (JWT, refresh)

UserService (CRUD профиля)

MatchService (лайки, проверка совпадений)

Kafka-пайплайн

⚙️ Пример архитектуры Kafka на свайп
```rust

Frontend ---> Gateway (REST/gRPC) ---> MatchService
                                       ↓
                            Kafka: "swipe.right"
                                       ↓
                            MatchService → "match.created"
                                       ↓
                       NotificationService, ChatService → уведомления / чат
```
