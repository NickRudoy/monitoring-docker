# Мониторинг-система на базе Docker

Проект представляет собой комплексное решение для мониторинга системы с использованием Grafana, Prometheus, GitLab и PlantUML.

## Компоненты системы

- **Grafana** - визуализация метрик
- **Prometheus** - сбор и хранение метрик
- **Node Exporter** - экспортер системных метрик
- **GitLab CE** - система управления репозиториями
- **PlantUML** - сервер для генерации UML диаграмм
- **Nginx** - прокси-сервер

## Требования

- Docker Engine
- Docker Compose
- Минимум 8GB RAM
- 20GB свободного места на диске

## Структура проекта 

```
├── docker-compose.yml
├── .env
├── configs
│ ├── grafana
│ │ └── provisioning
│ │ ├── dashboards
│ │ │ ├── dashboard.yml
│ │ │ └── os-general.json
│ │ └── datasources
│ │ └── prometheus.yml
│ ├── nginx
│ │ └── nginx.conf
│ └── prometheus
│ └── prometheus.yml
├── .gitignore
└── README.md
```

## Установка и запуск

1. Клонировать репозиторий:
```bash
git clone <repository-url>
cd <repository-name>
```

2. Добавить записи в файл hosts:
```
127.0.0.1 metrics.local
127.0.0.1 gitlab.local

- Windows: `C:\Windows\System32\drivers\etc\hosts`
- Linux/Mac: `/etc/hosts`
```
3. Запустить проект:
```
docker-compose up -d
```

## Доступ к сервисам

- **Grafana**: http://metrics.local
  - Логин: admin
  - Пароль: admin (можно изменить в файле .env)

- **GitLab**: http://gitlab.local
  - Логин: root
  - Для получения пароля выполните:
    ```bash
    docker exec -it <gitlab-container-id> grep 'Password:' /etc/gitlab/initial_root_password
    ```

- **PlantUML**: http://localhost:9999

## Мониторинг

### Dashboard OS General
Включает следующие метрики:
- Сетевой трафик (входящий/исходящий)
- Свободное место на диске
- Количество файловых дескрипторов

## Конфигурация

### Prometheus
- Интервал сбора метрик: 15 секунд
- Целевые эндпоинты настраиваются в `configs/prometheus/prometheus.yml`

### Grafana
- Автоматическое провизионирование дашбордов из `configs/grafana/provisioning/dashboards`
- Автоматическое подключение источника данных Prometheus

### Nginx
- Проксирование запросов к Grafana и GitLab
- Конфигурация в `configs/nginx/nginx.conf`

## Управление

### Запуск
```
docker-compose up -d
```

### Остановка
```
docker-compose down
```

### Просмотр логов
```bash
Все сервисы
docker-compose logs
Конкретный сервис
docker-compose logs <service-name>
```

### Полная очистка (включая volumes)
```bash
docker-compose down -v
```

## Мониторинг состояния

Проверка статуса сервисов:
```bash
docker-compose ps
```
