## 006-Writing a Docker Compose configuration

```
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

```
   docker-compose down  
```
Эта команда остановит и удалит все контейнеры и его образы. Она объединяет команды:
- stop - остановка контейнеров
- rm   - удаление образов

```
    docker-compose stop
```
Эта команда останавливает контейнеры.

```
    docker-compose restart
```
Эта команда остановит и запустит контейнеры. Она объединяет команды:
- stop
- start

```
    docker-compose --help
```
Эта команда отобразит список всех команд.

## 008-Build arguments

Указание аргументов (во время сборки, действуют не внутри контейнеров)
```
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

Переменные окружения (будут работать внутри контейнеров)
```
version: "3.9"

services:
  storefront: 
    build:
      context: .
      args:
        - region=us-east-1
        - alice=0
    environment:
      - runtime_env      # переменная берёт значение из ОС
      - runtime_env=dev  # переменная берёт значение здесь же
  database: 
    image: "mysql"
    env_file:  # указать файл с переменными
      - ./mysql/env_vars

```
## 009-Mounting volumes
```
version: "3.9"

services:
  ...
  database: 
    image: "mysql"
    env_file:  # указать файл с переменными
      - ./mysql/env_vars
    volumes:
      - ./mysql:/var/lib/mysql:ro
```
:ro - режим только чтение (для базы данных)  
по умолчанию работает rw  
:rw - режим записи чтения (для базы данных)  

## 010-Named volumes

Создание именованных томов (volumes)

```
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
Существует длинный и короткий синтаксис записи. Здесь разобран.  
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
```
version: "3.9"

services:
  scheduler:
    build: scheduler/.
    ports:
      - "81:80" # локально:в_контейнере
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
```
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
      - database
  database: 
    ...
volumes:
  kineteco: 
```

## 013-Named subsets of services

Профили. Профили предназначены для взаимосвязанного запуска контейнеров в условиях, когда несколько других взаимосвязанных контейнеров не подлежат запуску совсем.  
Для этого указываются профили и задаётся нужная команда. 
```
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
В этом случае запустится и database с указанным профилем. Либо database можно было так же добавить в профиль для запуска с соответствующим профилем:

    database:
      profiles:
        - storefront_services 
        - scheduling_services
            

Запуск командой:  

    docker-compose --profile storefront_services up

Могут быть так же запущены 
   
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

В docker-compose можно передать переменные, для удобства частой смены тех или иных настроек. Переменная может быть задана в среде хост-машины (главной машины) (она никогда не переопределяется другими другими способами), ин-лайн в docker-compose.yml, во внешнем файле (например, .env), или по требованию (чтоб переменная не была пустой). Если она передана, но нигде не указана, будет автоматически воспринята как пустая строка.

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
