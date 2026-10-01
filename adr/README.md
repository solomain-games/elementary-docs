# Архитектурные решения (ADR)

Architecture Decision Record — короткая запись об одном важном решении: почему оно принято, какие были варианты и чем придётся платить. Записи не переписываются задним числом: если решение меняется, создаётся новая запись, а старая получает статус «заменено».

Новая запись: скопировать [template.md](template.md) в `NNNN-kratkoe-nazvanie.md` со следующим номером.

| № | Решение | Статус |
|---|---|---|
| [0001](0001-rooms-in-game-service.md) | Комнаты живут в game-service | принято |
| [0002](0002-no-postgres-rabbitmq-in-mvp.md) | В MVP нет PostgreSQL и RabbitMQ | принято |
| [0003](0003-jwt-rs256-user-service.md) | JWT подписывает user-service (RS256) | принято |
| [0004](0004-game-core-pure-java.md) | game-core — чистая Java-библиотека | принято |
| [0005](0005-per-player-state-view.md) | Каждому игроку — его личное представление состояния | принято |
| [0006](0006-case-as-file-in-mvp.md) | Дело в MVP — файл в репозитории | принято |
| [0007](0007-single-vm-docker-compose.md) | Одна виртуальная машина с Docker Compose и Caddy | принято |
| [0008](0008-multirepo-with-prefix.md) | Отдельный репозиторий на сервис, префикс проекта | принято |
