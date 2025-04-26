# Развёртывание Telegram-бота с PostgreSQL в Kubernetes

## Структура проекта

- **oleynik-bot** - Docker-образ с Telegram-ботом.
- **oleynik-db** - Docker-образ с PostgreSQL базой данных.
- **Kubernetes манифесты** для развёртывания бота и базы данных.

## Шаги по развёртыванию

### 1. Подготовка Docker-образов

1. **Создание Docker-образа для PostgreSQL базы данных**:

    ```bash
    docker build -t cr.yandex/crpcojl564cc4biuq8nc/oleynik-db:latest .
    ```

    ```bash
    docker push cr.yandex/crpcojl564cc4biuq8nc/oleynik-db:latest
    ```

2. **Создание Docker-образа для Telegram-бота**:

    ```bash
    docker build -t cr.yandex/crpcojl564cc4biuq8nc/oleynik-bot:latest .
    ```

    ```bash
    docker push cr.yandex/crpcojl564cc4biuq8nc/oleynik-bot:latest
    ```

### 2. Создание пространства имён

    ```bash
    kubectl create namespace oleynik-ns
    ```

### 3. Развёртывание PostgreSQL базы данных

    ```bash
    kubectl apply -f database.yaml
    ```

### 4. Развёртывание Telegram-бота

    ```bash
    kubectl apply -f tripplanner.yaml
    ```

### 5. Проверка состояния

    ```bash
    kubectl get pods -n oleynik-ns
    ```

    ```bash
    kubectl logs -l app=oleynik-bot -n oleynik-ns --tail=100
    ```

### 5. Удаление всех ресурсов

```bash
kubectl delete -f tripplanner.yaml
kubectl delete -f database.yaml
