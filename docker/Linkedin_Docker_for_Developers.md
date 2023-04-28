# Linkedin Docker for Developers 2022

## 004-Base application setup

Установка с сайтов
- Docker
- Mongo DB community Edition  
  Я не устанавливал MongoDB как сервис (как службу Windows), т.е. снял галочку с "Install MongoDB as a Service"  
  Установил MongoDBCompass - это графический интерфейс.

Ссылки для Mongo

Установка  
https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-windows/

На этой же странице - запуск Монго, если Монго не устанавливался как служба  
https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-windows/#std-label-run-mongodb-from-cmd

## 005-Creating your first Docker image

Создать .dockerignore

Создать Dockerfile  
Dockerfile создаст image (образ), на основе которого будет создан контейнер.


## 006-Base commands exploration

После создания dockerfile команда для создания образа.  
-t задаёт имя.  
Согласно Dockerfile будет выгружен из онлайн образ согласно команде "From образ" и на основе остальных команд Dockerfile будет создан собственный локальный образ.

    docker build -t algofields/simple-backend .

Список всех образов

    docker images

Для удаления образа достаточно ввести первые 4-5 символов идентификатора (id) образа

    docker rmi 72ab4

Запуск контейнера (или приложения) на основе образа, команда будет выполняться без остановки (Ctrl+C для остановки)

    docker run -p 4000:4000 algofields/simple-backend

Список запущенных контейнеров 

    docker ps

Остановить контейнеры

    docker stop

или

    docker kill

Поставка и получение образов в онлайн

    docker pull    # извлечь из хаба hub.docker.com

    docker push    # поместить в хаб hub.docker.com


Ещё команды 

https://docs.docker.com/engine/reference/commandline/container/

## 008

Создать docker-compose.yml

## 009 Simple-backend

Построить все образы

    docker-compose build

Запустить определенный контейнер (service) по имени из docker-compose.yml.  
 Полезно при определённой последовательности запуска контейнеров.

    docker-compose up -d mongo
    docker-compose up -d app

Посмотреть логи контейнера по id

    docker logs 01ab4

Остановить все контейнеры

    docker-compose stop

## 011 Frontend

Создание образа

    docker build -t algofields/frontend .

Список всех образов

    docker images

Создание (нового!) контейнера на основе образа (каждый раз создаётся новый контейнер этой командой)

    docker run -p 3000:3000 algofields/frontend

Удаление образа

    docker rmi 72ab4

Остановка контейнера

    docker stop 72ab4

Запущенные контейнеры

    docker ps

Eще команды

https://docs.docker.com/engine/reference/commandline/docker/


---
Проблема команды npm install из Dockerfile, из-за чего не запускался контейнер на основе образа, была решена иизменением в package.json.

Вместо  

    "start": "react-scripts start"

Нужно  

    "start": "react-scripts --openssl-legacy-provider start"

Проблема вызывается из-за залатывания разработчиками дыр в Node.js из-за SSL.

https://stackoverflow.com/questions/69692842/error-message-error0308010cdigital-envelope-routinesunsupported

---

## 012 Fullstack

    docker-compose build

    docker-compose up -d mongo  
    docker-compose up -d app
    docker-compose up -d client 

    docker ps

    docker-compose stop

## 015 Create a Swarm

    docker swarm init

    docker info

    docker node ls

## 016 Adding nodes to the swarm

Убедиться, что мы в папке с файлом docker-compose.yml

    ls

Команда для создания стэка

    docker stack deploy -c docker-compose.yml имя_стэка_(желаемое?)

## 017-Deploy and inspect a service to the swarm

--replicas - это количество сервисов, которые нужно добавить.  
--name nodeserver2 - желаемое название

    docker service create --replicas 1 --name nodeserver2 node ping docker.com

    docker servise ls

## 019-Installing Kubernetes

kubernetes.io/releases/download/

    brew install kubectl

    kubectl version --client

minikube.sigs.k8s.io/docs

    brew install minikube

## 020-Creating your first cluster

Создать кластер

    minikube start

    kubectl cluster-info

## 021-Deploy your first app to the cluster

    kubectl create deployment wanted_name --image=wanted_image

      or image from link

    kubectl create deployment wanted_name --image=http://google.com/node

Список созданного

    kubectl get deployments

---



    




