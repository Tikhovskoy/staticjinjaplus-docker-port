# StaticJinjaPlus Docker Port

Этот репозиторий позволяет собирать Docker-образы для утилиты [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus).

Образы можно:
- использовать сразу с Docker Hub;
- собрать самостоятельно с нужной версией и на нужной базе.

---

## Готовые образы

Все образы опубликованы в [Docker Hub](https://hub.docker.com/r/tikhovskoi/static-jinja-plus):

| Версия StaticJinjaPlus | Базовый образ   | Тег образа                            |
|------------------------|-----------------|---------------------------------------|
| `0.1.0`                | `ubuntu`        | `tikhovskoi/static-jinja-plus:0.1.0`  |
| `0.1.0`                | `python:slim`   | `tikhovskoi/static-jinja-plus:0.1.0-slim` |
| `0.1.1`                | `ubuntu`        | `tikhovskoi/static-jinja-plus:0.1.1`  |
| `0.1.1`                | `python:slim`   | `tikhovskoi/static-jinja-plus:0.1.1-slim` |
| `develop`             | `ubuntu`        | `tikhovskoi/static-jinja-plus:develop` |
| `develop`             | `python:slim`   | `tikhovskoi/static-jinja-plus:develop-slim` |
| Последняя стабильная | `ubuntu`        | `tikhovskoi/static-jinja-plus:latest` |
| Последняя стабильная | `python:slim`   | `tikhovskoi/static-jinja-plus:slim`   |

---

## Быстрый старт: запуск контейнера

Создайте папку с шаблонами Jinja2:

```bash
mkdir my_templates output
echo "<h1>Hello from StaticJinjaPlus</h1>" > my_templates/index.html
````

Запустите генерацию HTML:

```bash
docker run --rm \
  -v "$PWD/my_templates:/tmp/templates:ro" \
  -v "$PWD/output:/tmp/render" \
  tikhovskoi/static-jinja-plus:0.1.1
```

Проверьте результат:

```bash
cat output/index.html
```

---

## Возможности образа

Каждый контейнер поддерживает:

* генерацию (`staticjinja build`)
* отслеживание изменений (`staticjinja watch`)
* параметризацию путей

Пример для режима `watch`:

```bash
docker run --rm \
  -v "$PWD/my_templates:/tmp/templates:ro" \
  -v "$PWD/output:/tmp/render" \
  tikhovskoi/static-jinja-plus:0.1.1 \
  staticjinja watch --srcpath /tmp/templates --outpath /tmp/render
```

---

## Сборка образа самостоятельно

Вы можете собрать Docker-образ из любой версии StaticJinjaPlus.

### 1. Скачайте архив и получите хеш

```bash
wget https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.1.tar.gz -O SJP.tar.gz
sha256sum SJP.tar.gz
```

Скопируйте полученный SHA256-хеш.

---

### 2. Сборка с Ubuntu

```bash
docker build -f Dockerfile.ubuntu \
  --build-arg SJP_VERSION=0.1.1 \
  --build-arg HASH=<вставьте_хеш_сюда> \
  -t yourname/static-jinja-plus:0.1.1 .
```

---

### 3. Сборка с Python Slim

```bash
docker build -f Dockerfile.slim \
  --build-arg SJP_VERSION=0.1.1 \
  --build-arg HASH=<вставьте_хеш_сюда> \
  -t yourname/static-jinja-plus:0.1.1-slim .
```

---

### 4. Сборка develop-версии (ветка `main`)

```bash
wget https://github.com/MrDave/StaticJinjaPlus/archive/refs/heads/main.tar.gz -O main.tar.gz
sha256sum main.tar.gz
```

```bash
docker build -f Dockerfile.slim \
  --build-arg SJP_VERSION=heads/main \
  --build-arg HASH=<хеш_main> \
  -t yourname/static-jinja-plus:develop-slim .
```

---

## Проверка результата

После сборки вы можете протестировать образ так:

```bash
mkdir my_templates output
echo "<h1>Hello</h1>" > my_templates/index.html

docker run --rm \
  -v "$PWD/my_templates:/tmp/templates:ro" \
  -v "$PWD/output:/tmp/render" \
  yourname/static-jinja-plus:0.1.1

cat output/index.html
```

Если всё прошло успешно, в `output/index.html` появится содержимое шаблона.

---

## Структура репозитория

```
.
├── Dockerfile.ubuntu       # Dockerfile на базе Ubuntu
├── Dockerfile.slim         # Dockerfile на базе python:slim
└── README.md               # Этот файл
```

---

## Важно

* В Dockerfile используется `ADD --checksum`, поэтому вы **обязательно** должны указать правильный SHA256-хеш архива при сборке.
* Образы `latest` и `slim` соответствуют последней стабильной версии `0.1.1`.
* Образы `develop` и `develop-slim` соответствуют последнему коммиту из ветки `main`.

---

## Полезные ссылки

* [StaticJinjaPlus на GitHub](https://github.com/MrDave/StaticJinjaPlus)
* [Документация StaticJinjaPlus](https://staticjinja.readthedocs.io/)
* [Docker Hub tikhovskoi/static-jinja-plus](https://hub.docker.com/r/tikhovskoi/static-jinja-plus)
