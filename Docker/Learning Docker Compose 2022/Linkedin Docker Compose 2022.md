# 2. Getting Started with Docker Compose

## 006-Writing a Docker Compose configuration

```yaml
version: "3.9"

services:
  storefront: 
    build: .  # указывается путь к Dockerfile
  database: 
    image: "mysql"
```

## 007-Core Docker Compose commands

    docker-compose up (-d, --detach)

docker-compose up объединяет три команды:
- build  - для создания образов
- create - для создания контейнеров
- start  - для запуска контейнеров  


Можно запускать отдельные контейнеры, а не все, например:  

    docker-compose up storefront

---

```
   docker-compose down  
```
Эта команда остановит и удалит все контейнеры и его образы. Она объединяет команды:
- stop - остановка контейнеров
- rm   - удаление образов
---

```
    docker-compose stop
```
Эта команда останавливает контейнеры.

---

```
    docker-compose restart
```
Эта команда остановит и запустит контейнеры. Она объединяет команды:
- stop
- start
---

```
    docker-compose --help
```
Эта команда отобразит список всех команд.

---

# 3. Docker Compose Core Features

## 008-Build arguments

Указание аргументов (во время сборки, действуют не внутри контейнеров)
```yaml
version: "3.9"

services:
  storefront: 
    build:
      context: .
      args:
        - region=us-east-1
        - alice=0
  database: 
    image: "mysql"
```

Переменные сборки build будут работать только во время сборки.  
Переменные окружения environment будут работать внутри контейнеров.  

```yaml
version: "3.9"

services:
  storefront: 
    build:
      context: . # путь к Dockerfile
      args:
        - region=us-east-1  # переменная сборки
        - alice=0           # переменная сборки
    environment:
      - runtime_env      # такая переменная берёт значение из ОС
      - runtime_env=dev  # такая переменная берёт значение отсюда же
  database: 
    image: "mysql"
    env_file:         # файл с переменными вместо перечисления переменных
      - ./mysql/env_vars

```
## 009-Mounting volumes

Создание томов в контейнерах (services) для хранения информации вне неработающего контейнера. При запуске docker-compose эти данные передаются с хоста в контейнер.  

Источник - место хранения данных на хосте (выбираем сами), цель - место хранения данных в контейнере (обычно надо выбрать место по умолчанию).

```
  volume:
     source_path_out_container:target_path_in_container
```

```yaml
version: "3.9"

services:
  ...
  database: 
    image: "mysql"
    env_file:  # файл с переменными
      - ./mysql/env_vars
    volumes:
      - ./mysql:/var/lib/mysql:ro
```

./mysql - папка в директории с файлом docker-compose.yaml

/var/lib/mysql - папка в контейнере

> Access Modes

Модификаторы доступа для томов:

:ro - режим только чтение, read-only  
:rw - режим записи и чтения, read-write  

по умолчанию работает rw  

## 010-Named volumes

Создание именованного тома kineteco:  

```yaml
version: "3.9"

services:
  ...
  database: 
    image: "mysql"
    env_file:  # указать файл с переменными
      - ./mysql/env_vars
    volumes:
      - ./mysql:/var/lib/mysql:ro
      - kineteco:/var/lib/mysql
volumes:
  kineteco:
```

Команда 

    docker-compose down -- volumes

удалит все именованные тома.  

Существует длинный и короткий синтаксис записи.   
Короткий:

    - kineteco:/var/lib/mysql:rw

Длинный:
```
   type: volume
   source: kineteco
   target: /var/lib/mysql
   read_only: false

```

## 011-Exposing ports

Проброс портов
```yaml
version: "3.9"

services:
  scheduler:
    build: scheduler/.
    ports:
      - "81:80"    # локально:в_контейнере
  storefront: 
    build: storefront/.
    ports:
      - "80:80"    # локально:в_контейнере
      - "443:443"  # локально:в_контейнере
  database: 
    image: "mysql"
    env_file:
      - ./mysql/env_vars
    volumes:
      - ./mysql:/docker-entrypoint-initdb.d:ro
      - kineteco:/var/lib/mysql
volumes:
  kineteco: 
```
## 012-Enforcing start up order

Запуск чего-то первым
```yaml
version: "3.9"

services:
  scheduler:
    ...
  storefront: 
    build: storefront/.
    ports:
      - "80:80"
      - "443:443"
    depends_on:   # зависимость от
      - database  # перед storefront будет сначала запущена база данных
  database: 
    ...
volumes:
  kineteco: 
```

# 4. Dynamic Configurations in Docker Compose

## 013-Named subsets of services

Профили.  
Профили предназначены для взаимосвязанного запуска контейнеров в условиях, когда несколько других взаимосвязанных контейнеров не подлежат запуску совсем.  
Для этого указываются профили и задаётся нужная команда. 

Например, команда  

    docker-compose --profile storefront_services up

запустит все контейнеры, где указан этот профиль storefront_services и где не указан никакой профиль.  
В следующем примере - это контейнер storefront и database .  

```yaml
version: "3.9"

services:
  scheduler:    
    ...
    depends_on:
      - database
    profiles:
      - scheduling_services
  storefront:
    ...
    depends_on:
      - database
    profiles:
      - storefront_services
  database: 
    ...
```

Либо database можно было явно добавить в профиль для запуска с соответствующим профилем, перечислив, нпример, их оба:

    database:
      profiles:
        - storefront_services 
        - scheduling_services

Также, помимо up, с указанием профиля могут быть запущены команды
   
    ...down 
    ...stop
    ...restart

## 014-Multiple compose files

Можно создать дополнительные файлы docker-compose для переопределения или дополнения настроек запуска, используемых в основном файле по умолчанию.  
Команда, требуемая для запуска таких файлов:  

    # -f or --file
    # [command] - up, stop, etc.

    docker-compose -f [primary_file] -f [override_file] [command]

Например,

    docker-compose -f docker-compose.yaml -f docker-compose.local.yaml up

## 015-Environment variables

В docker-compose можно передать переменные, для удобства частой смены тех или иных настроек. Переменная может быть задана в среде хост-машины (главной машины) (она никогда не переопределяется другими способами), ин-лайн в docker-compose.yml, во внешнем файле (например, .env), или по требованию (чтоб переменная не была пустой). Если она передана, но нигде не указана, будет автоматически воспринята как пустая строка.

Инлайн
```
version: "3.9"

services:
  scheduler:
    ...
  storefront: 
    ...
  database: 
    image: "mysql:-latest"
    ...
volumes:
  kineteco:

```

env file

```
version: "3.9"

services:
  scheduler:
    ...
  storefront: 
    ...
  database: 
    image: "mysql:${TAG}"
    ...
volumes:
  kineteco:
```
env:

    TAG=latest

Запуск env-файла за пределами папки проекта

    docker-compose --env-file [path] [up, down, stop, restart]

Вариант с требованием переменной (на это указывает вопросительный знак) и сообщением об ошибке (после вопросительного знака):

```
version: "3.9"

services:
  scheduler:
    ...
  storefront: 
    ...
  database: 
    image: "mysql:?oops TAG is requred"
    ...
volumes:
  kineteco:
```

----
