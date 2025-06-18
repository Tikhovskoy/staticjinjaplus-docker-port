# StaticJinjaPlus Docker Port

Этот репозиторий содержит Dockerfile'ы для сборки образов [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus).

## Доступные теги

| Версия | База        | Тег Docker Hub                            |
|--------|-------------|-------------------------------------------|
| 0.1.0  | ubuntu      | `tikhovskoi/static-jinja-plus:0.1.0`      |
| 0.1.0  | python-slim | `tikhovskoi/static-jinja-plus:0.1.0-slim` |
| 0.1.1  | ubuntu      | `tikhovskoi/static-jinja-plus:0.1.1`      |
| 0.1.1  | python-slim | `tikhovskoi/static-jinja-plus:0.1.1-slim` |
| develop | ubuntu     | `tikhovskoi/static-jinja-plus:develop`    |
| develop | python-slim | `tikhovskoi/static-jinja-plus:develop-slim` |
| latest | ubuntu      | `tikhovskoi/static-jinja-plus:latest`     |
| slim   | python-slim | `tikhovskoi/static-jinja-plus:slim`       |

## Как собрать

Пример сборки версии 0.1.1:

```bash
docker build -f Dockerfile.ubuntu \
  --build-arg SJP_VERSION=0.1.1 \
  --build-arg HASH=<sha256-хэш> \
  -t tikhovskoi/static-jinja-plus:0.1.1 .
````

Для slim-версии:

```bash
docker build -f Dockerfile.slim \
  --build-arg SJP_VERSION=0.1.1 \
  --build-arg HASH=<sha256-хэш> \
  -t tikhovskoi/static-jinja-plus:0.1.1-slim .
```

## Зачем это нужно

* Выбирайте конкретную версию генератора
* Используйте лёгкие образы на `slim`-базе
* Поддержка будущих версий без ручного обновления
