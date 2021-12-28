---
layout: default
title: Box
nav_order: 3
has_children: true
---

Этот Docker-контейнер представляет из себя имплементацию [интеграционной шины](https://en.wikipedia.org/wiki/Enterprise_service_bus).
Является тонким клиентом для проксирования запросов на приватные API.

Установка
============

```bash
docker run \
-p 80:80/tcp \
-e BOX_ADMIN_USER=user \
-e BOX_ADMIN_SECRET=secret \
-e PG_REAL_HOST=db \
-e PG_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=box_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-e QUEUE_URL=http://queue \
-e QUEUE_BACK_URL=http://box \
-e QUEUE_WORKER=box \
-d images.perfumerlabs.com/dist/box:v3.2.1
```

Переменные окружения
=====================

- BOX_ADMIN_USER - имя пользователя с админскими парвами, который будет создан при первой загрузке. Обязательно.
- BOX_ADMIN_SECRET - пароль пользователя с админскими парвами, который будет создан при первой загрузке. Обязательно.
- BOX_FETCH_LIMIT - максимальное число документов, возвращаемое в методе "/documents". По умолчанию 100.
- BOX_IS_SYSTEM - определяет, является ли инстанс системным, т.е. предназначенным для синхррнизации данных. По умолчанию false.
- BOX_INSTANCES - Перечисленные через запятую инстансы, с которых нужно синхронизировать данные. Обязательно, если BOX_IS_SYSTEM=true.
- PG_HOST - PostgreSQL инстанс для подключения. Обязательно.
- PG_REAL_HOST - адрес базы данных PostgreSQL (НЕ PgBouncer). Обязательно.
- PG_SLAVES - перечисленные через запятую инстансы для slave-соединений. Опционально.
- PG_PORT - порт PostgreSQL. По умолчанию 5432.
- PG_DATABASE - название базы данных PostgreSQL. Обязательно.
- PG_SCHEMA - схема базы данных PostgreSQL. По умолчанию "public".
- PG_USER - пользователь для подключения к PostgreSQL. Обязательно.
- PG_PASSWORD - пароль пользователя PostgreSQL. Обязательно.
- QUEUE_URL - адрес очереди [Queue](https://github.com/perfumerlabs/queue). Обязательно.
- QUEUE_BACK_URL - обратный адрес на шину, на которую очередь будет делать запрос. Обязательно.
- QUEUE_WORKER - название воркера на очереди, сконфигурированного для шины. Обязательно.
- PHP_PM_MAX_CHILDREN - число PHP FPM процессов. По умолчанию 10.
- PHP_PM_MAX_REQUESTS - лимит запросов на 1 процесс, после которого он перезагружается. По умолчанию 500.

Volumes
=======

Контейнер не содержит volumes.

Если требуется дополнительная конфигурация контейнера, установите bash-скрипт в /opt/setup.sh.
Этот скрипт выполнится при первой загрузке.

Доступы и авторизация
=====================

При первой загрузке контейнера будет создан пользователь с атрибутами, указанными в переменных `BOX_ADMIN_USER` и `BOX_ADMIN_SECRET`.
Он обладает администраторскими правами.
В дальнейшем можно создавать других пользователями и наделять их правами.

При создании пользователя, ему выдается специальный секрет.
Каждый запрос на API шины должен содержать заголовок Api-Secret со значением этого секрета. Например:

```
Api-Secret: my_secret
```

Коллекции
=========

Перед тем как работать с шиной, необходимо создать коллекции.
Коллекция - это отдельная таблица в базе данных, содержащая документы, которые сохраняют пользователи.
Необходимо создавать коллекцию на каждый отдельный сервис шины.

На каждую коллекцию отдельным пользователям возможно настроить доступы: только чтение из коллекции; запись и чтение.
Администраторы могут как писать, так и читать из любой коллекции.

Чтобы создать коллекцию надо вызвать следующий метод API (действие доступно только администраторам):

`POST /collection`

JSON-параметры (json):
- name [string,required] - название коллекции (строго из маленьких латинских букв или цифр).
- type [string,required] - режим коллекции: "sync", "async", "storage".
- handler [string,optional] - URL обработчика запросов. Для режимов коллекции "sync" и "async".

Пример:

```json
{
    "name": "foobar",
    "type": "sync",
    "handler": "https://example.com"
}
```

Пример ответа:

```json
{
    "status": true
}
```

Чтобы выдать доступ к коллекции пользователю, надо вызвать метод API (действие доступно только администраторам):

`POST /access`

JSON-параметры:
- client [int,required] - ID пользователя.
- collection [int,required] - ID коллекции.
- level [string,required] - Доступ: "read" (только чтение) или "write" (чтение и запись).

Пример:

```json
{
    "client": 2,
    "collection": 1,
    "level": "write"
}
```

Пример ответа:

```json
{
    "status": true
}
```

Режимы коллекции
==================

При создании коллекции можно указать режим работы. Всего доступно 3 режима: синхронный, асинхронный, промежуточное хранилище.

#### Cинхронный режим

Для создания такого типа коллекции необходимо передать при создании параметр "type": "sync".
Также обязательно указать "handler".

При запросе на такую коллекцию запрос будет автоматически перенаправлен на "handler" с данными переданными в запросе.
Например:

Пользователь, имеющий доступ на запись в коллекцию делает запрос:

`POST /document`

JSON-параметры:
- collection [string,required] - Название коллекции.
- event [string,required] - название события/подсервиса/подколлекции.
- uuid [string,required] - сгенерированный UUID.
- data [mixed,required] - Данные для отправки на сервис.

Пример запроса:

```json
{
    "collection": "my_service",
    "event": "my_event",
    "uuid": "123e4567-e89b-12d3-a456-426614174000",
    "data": "Lorem ipsum dolor sit amet"
}
```

Шина сделает у себя внутри запрос на "handler" указанный при создании коллекции и вернет ответ:

JSON-параметры ответа:
- id [integer] - ID созданного документа.
- status_code [integer] - Статус кода ответа сервиса.
- body [mixed] - контент возвращенный сервисом.

Пример ответа:

```json
{
    "status": true,
    "content": {
        "document": {
            "id": 1
        },
        "response": {
            "status_code": 201,
            "body": "data from service"
        }
    }
}
```

#### Асинхронный режим

Для создания такого типа коллекции необходимо передать при создании параметр "type": "async".
Также обязательно указать "handler".

Асинхронный режим работает аналогично синхронному.
Разница в том, что асинхронная коллекция сразу не возвращает ответ от сервиса, а делает это позже на переданный webhook.
Например:

Пользователь, имеющий доступ на запись в коллекцию делает запрос:

`POST /document`

JSON-параметры:
- collection [string,required] - Название коллекции.
- event [string,required] - название события/подсервиса/подколлекции.
- uuid [string,required] - сгенерированный UUID.
- data [mixed,required] - Данные для отправки на сервис.
- webhook [string,required] - URL, на который необходимо вернуть результат запроса на сервис.

Пример запроса:

```json
{
    "collection": "my_service",
    "event": "my_event",
    "uuid": "123e4567-e89b-12d3-a456-426614174000",
    "data": "Lorem ipsum dolor sit amet",
    "webhook": "https://my-site.com"
}
```

Шина сохранит документ и вернет сохраненный ID:

JSON-параметры ответа:
- id [integer] - ID созданного документа.

Пример ответа:

```json
{
    "status": true,
    "content": {
        "document": {
            "id": 1
        }
    }
}
```

Далее шина внутри себя через механизиы очередей делает запрос на сервис.
В случае если сервис не отвечает, то шина будет стараться повторить запрос позже.
При успешном ответе от сервиса шина вернет результат на указанный в документе webhook (структура аналогична синхронному ответу):

Пример запроса на webhook:

```json
{
    "status": true,
    "content": {
        "document": {
            "id": 1
        },
        "response": {
            "status_code": 201,
            "body": "data from service"
        }
    }
}
```

#### Режим внутреннего хранилища

Данный режим предназначен для случаев, когда пользователи будут самостоятельно забирать данные из шины.
Пользователь, имеющий доступ на запись, создает документы в шине по заранее согласованному формату, когда в его системе наступают некие события.
Другой пользователь, имеющий доступ на чтение, периодически проверяет наличие новых документов и сохраняет их в своей системе.

Например, создается документ:

`POST /document`

JSON-параметры:
- collection [string,required] - Название коллекции.
- event [string,required] - название события/подсервиса/подколлекции.
- uuid [string,required] - сгенерированный UUID.
- data [mixed,required] - Данные для отправки на сервис.

Пример запроса:

```json
{
    "collection": "my_service",
    "event": "my_event",
    "uuid": "123e4567-e89b-12d3-a456-426614174000",
    "data": "Lorem ipsum dolor sit amet"
}
```

Другой пользователь, который хочет получить документы делает запросы:

`GET /documents`

Request parameters (json):
- collection [string,required] - Название коллекции.
- id [integer,optional] - ID документа от которого получить новые документы.
- limit [integer,optional] - лимит документов для получения.

Пример запроса:

```json
{
    "collection": "foobar",
    "id": 100
}
```

параметры ответа (json):
- event [string] - название события/подсервиса/подколлекции.
- uuid [string] - UUID идентификатор.
- id [integer] - ID документа.
- data [mixed] - контент документа.

Пример ответа:
```json
{
    "status": true,
    "content": {
        "documents": [
            {
                "id": 101,
                "event": "my_event",
                "uuid": "uuid",
                "data": [1, 2, 3]
            }
        ]
    }
}
```

Также можно получить число новых документов начиная с некоторого ID:

`GET /documents/count`

Request parameters (json):
- collection [string,required] - Название коллекции.
- id [integer,optional] - ID документа от которого получить новые документы.

Пример запроса:

```json
{
    "collection": "foobar",
    "id": 100
}
```

Параметры ответа (json):
- documents [int] - число найденных документов.

Пример ответа:

```json
{
    "status": true,
    "content": {
        "documents": 5
    }
}
```

High Availability
====================

В случае, если требуется HA (High Availability), т.е. автоматический failover, если сервера шины становятся недоступными, необходимо сделать следующую настройку.

Допустим, у нас есть 2 кластера Cluster1 и Cluster2.

На Cluster1 разворачивается необходимое количество Master-Slave баз данных Postgresql,
а также нужное количество контейнеров шины со следующим конфигом:

```bash
docker run \
-p 80:80/tcp \
-e BOX_ADMIN_USER=user \
-e BOX_ADMIN_SECRET=secret \
-e BOX_IS_SYSTEM=true \
-e BOX_INSTANCES=http://esb2 \
-e PG_REAL_HOST=db \
-e PG_HOST=db \
-e PG_PORT=5432 \
-e PG_DATABASE=box_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-e QUEUE_URL=http://queue \
-e QUEUE_BACK_URL=http://box \
-e QUEUE_WORKER=box \
-d docker/image
```

Ключевыми параметрами здесь являются BOX_IS_SYSTEM и BOX_INSTANCES.
BOX_IS_SYSTEM показывает, что этому контейнеру надо производить синхронизацию данных.
BOX_INSTANCES - указывается адрес кластера Cluster2.
При установке таких параметров контейнер будет сихронизировать схему коллекций (кроме документов), клиентов и доступы с Cluster2.

Аналогично на Cluster2 разворачивается необходимое количество своих Master-Slave баз данных Postgresql,
а также необходимо поднимать контейнеры со следующей конфигурацией:

```bash
docker run \
-p 80:80/tcp \
-e BOX_ADMIN_USER=user \
-e BOX_ADMIN_SECRET=secret \
-e BOX_IS_SYSTEM=true \
-e BOX_INSTANCES=http://esb \
-e PG_REAL_HOST=db2 \
-e PG_HOST=db2 \
-e PG_PORT=5432 \
-e PG_DATABASE=box_db \
-e PG_USER=user \
-e PG_PASSWORD=password \
-e QUEUE_URL=http://queue2 \
-e QUEUE_BACK_URL=http://box2 \
-e QUEUE_WORKER=box \
-d docker/image
```

где в BOX_INSTANCES указывается адрес кластера Cluster1.
