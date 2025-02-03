## Exercises files

https://github.com/LinkedInLearning/symfony-6-essential-training-4404688

# 1. The Symfony Project

## 004-Symfony components

https://symfony.com/components

## 005-Symfony as a framework

Установка:

https://symfony.com/doc/current/setup.html

Установка API приложения в папку example-api

    composer create-project symfony/skeleton:"6.3.*" example-api

Сейчас "7.2.x" .

Предлагается поставить "6.4.x", т.к. она - LTS.   
https://symfony.com/releases  
Requires PHP 8.1.0 or higher

Если установка не завершается из-за проблем с сертификатом SSL, то перезагрузить контейнеры Docker или локальную среду OpenServer.

Превращение API-приложения в web-приложение

    cd example-api
    composer require webapp

Не был указан поставщик vendor рядом с именем webapp. Это происходит благодаря устанавливаемому по умолчанию компоненту Flex:

https://symfony.com/doc/current/setup.html#installing-packages

## 006-The Symfony CLI

Известные уязвимости в PHP и фреймворках:  

https://github.com/FriendsOfPHP/security-advisories

## 007-Symfony project structure

Лучшие практики для папок:

https://symfony.com/doc/current/best_practices.html

Демо-проект для изучения структуры каталогов:  

https://github.com/symfony/demo

Requirements:  
PHP 8.2.0 or higher;  
PDO-SQLite PHP extension enabled;   

## 008-Understanding versions in Symfony

https://symfony.com/releases

https://symfony.com/doc/current/contributing/community/releases.html

## 009-Using Symfony documentation

https://symfony.com/doc/current/index.html

https://symfony.com/book

---
# 2. Developing in Symfony

## 010-Local development

Symfony server

https://symfony.com/doc/current/setup/symfony_server.html

MAMP - локальный веб-сервер

https://www.mamp.info/en/windows/

## 011-Using config files

Докуметация по конфиг файлам  

https://symfony.com/doc/current/configuration.html

## 12