## Установка Laravel 5.8

В содержании:
- Обсерверы
- Очереди
- и другое
  
  
> Vagrant

Под Vagrant потребуется 2 файла:
- Vagrantfile и 
- bootstrap.sh 

На основе bootstrap.sh  
в /etc/apache2/sites-enabled/default.conf автоматически будет настроено
- имя сайта (оно может иметь дополнительный префикс),
- корневая папка сайта.

В Windows имя сайта также нужно прописать в hosts .

> Запуск: vagrant up

Если потребуется старая версия composer (не понадобилась):  
https://stackoverflow.com/questions/64597051/how-to-downgrade-or-install-a-specific-version-of-composer

> Проверка сайта в браузере (подгружается из корневой папки)

> Установка Laravel

Этой командой из текущей папки устанавливается версия 5.8 в пустую подпапку (blog), она создастся автоматически.
    
    composer create-project --prefer-dist laravel/laravel blog "5.8.*"

После чего нужно вырезать и вставить все файлы в верхнюю папку из подпапки.  
По идее всегда устанавливается в подпапку либо (если не указать подпапку) установится в подпапку "laravel".

> Проверка в браузере

> Разрешения для папок
    
    sudo chmod 777 -R storage && sudo chmod 777 -R bootstrap/cache

Vagrant и VirtualBox жестко привязывают права к общей с Windows папке и ее вложенностям. Поменять права вряд ли удастся.

> Установка дебаг-бара

Будет прописан в /vendor
    
При ошибке установки barryvdh/laravel-debugbar на Laravel 5.8 командой

        composer require barryvdh/laravel-debugbar --dev 

встала версия 3.0

        composer require barryvdh/laravel-debugbar:~3.0 --dev

 Debugbar включается/отключается в файле .env

    APP_DEBUG=true
        или
    APP_DEBUG=false

> Подстройка под MySQL
 
 Для MySQL/MariaDB нужно будет добавить следующие строки в  
 app\Providers\AppServiceProvider.php,  
    описание здесь:  
    Index Lengths & MySQL / MariaDB
    https://laravel.com/docs/5.7/migrations#indexes https://laravel.com/docs/8.x/migrations#indexes :

    
    ...
    use Illuminate\Support\Facades\Schema;
    ...    
    public function boot()
    {
        Schema::defaultStringLength(191);
    }

> Создание нового соединения в Workbench

Заполнить:
- connection name: любое
- connection method: Standart TCP/IP over SSH
- SSH hostname: 127.0.0.1:2222 (Вагрант подключается через :2222)
- SSH username: vagrant (это особенность Вагранта, при подключении командой - vagrant ssh)
- SSH keyfile: отсутствует
- MySQL hostname: 127.0.0.1
- MySQL server port: 3306
- username: poligon58 (создавался в bootstrap.sh как $PROJECT_SLUG)

Вход через SSH в MySQL: 
- vagrant vagrant (особенности Вагранта по SSH)
- poligon58 poligon58 (особенности настройки $PROJECT_SLUG в bootstrap.sh в строке # Create a database user)

> Создание базы данных

Она уже создана из bootstrap.sh. Использовалась строка типа
    
    CREATE SCHEMA `poligon` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    
Кодировка utf8mb4 описана здесь:

    Index Lengths & MySQL / MariaDB
        https://laravel.com/docs/5.7/migrations#indexes :
    Laravel uses the utf8mb4 character set by default

> Настройка .env файла

    DB_DATABASE=poligon58
    DB_USERNAME=poligon58
    DB_PASSWORD=poligon58

Значения заданы с помощью $PROJECT_SLUG в bootstrap.sh
<hr>

## Git, начало, кратко

> Отключение папок в .gitignore

    _example.md
    /.vagrant

    # /storage
    # /bootstrap/cache
    # эти две папки не нужно отключать  
    1 - это вызовет проблемы при клонировании  
    2 - самой проблемной частью в них можно назвать storage\logs и storage\debugbar,  
    однако умные разработчики уже обо всём позаботились  
    и включили в эти и другие папки локальные файлы .gitignore,  
    чтобы в гит, а потом при клонировании, не подтягивались логи и дополнительные файлы,  
    но при этом сохранилась полноценная работоспособность фреймворка.

> git init

> git add .

> git commit -m "..."

> git tag -a 0.1

После создания удаленного репозитория:

> git remote add origin https://github.com/...

> git push origin master

> git push origin 0.1

> git branch develop

> git checkout develop

---

## 36. Перенос проекта из 5.7 в 5.8

Создание доп. файлов:
> php artisan make:auth

Перенос модели User.php в созданную папку app/Models

Создать репозиторий

Внедрение старого кода.  
Происходит копированием с заменой папок со старого проекта. Почти все файлы Modified (изменения 5.7, внедренные в 5.8) отменяем, за исключением влияющих на проект (например, database\seeds\DatabaseSeeder.php , routes\web.php). Все Unstaged (со старого проекта, новые созданные в проекте 5.7 файлы) коммитим.
- залить папку app
- залить папку database (база данных сейчас пустая)
- залить папку routes
- залить папку resources/views
- данные .env файла переносим руками

Возможно, потребуется (автокомплит классов в Laravel) 

    php composer.phar require barryvdh/laravel-ide-helper --dev

Создание БД  
(можно в файлах миграции подправить increments на bigIncrements и integer на bigInteger: особенность 5.8 )

    // Запускаем миграции и сиды одной командой
    php artisan migrate --seed

При ошибке команды можно удалить созданные таблицы перед следующим запуском команды.

Возможен последовательный запуск двумя командами:

    // создание таблиц
    php artisan migrate

    // заполнение таблиц
    php artisan db:seed

При ошибке "Класс не существует":

    composer dump-autoload
    // затем
    php artisan db:seed

Миграции - это описание столбцов таблиц  
Сидеры и Фабрики - чем заполнять таблицы  
database\seeds\DatabaseSeeder@run - в какой последовательности заполнять БД

Версия Laravel
> php artisan -V

---
## 39.

Список всех существующих маршрутов  

    php artisan route:list

При добавлении новых классов в файлы - не забывать прописывать use .

---
## 40. Обновление поста

Создать реквест BlogPostUpdateRequest:
создание своего класса запросов, расширяющего FormRequest,
попадает в папку app/Http/Requests, к-рая будет создана (если её не было)

    php artisan make:request BlogPostUpdateRequest

---
## 41. Обсервер

    php artisan make:observer BlogPostObserver --model=Models\BlogPost
    php artisan make:observer BlogCategoryObserver --model=Models\BlogCategory

Создаёт немного неправильные обсерверы (ModelsBlogPost)
Правильней, видимо

    php artisan make:observer BlogPostObserver --model=BlogPost
    php artisan make:observer BlogCategoryObserver --model=BlogCategory

Код с классами обсервера добавляется в AppServiceProvider

---
## 43. 

    php artisan make:request BlogPostCreateRequest

---
## 48.

    php artisan make:controller DiggingDeeperController

---
## 51. Update Laravel 5.8->6.0

Замена строк composer.json из 6,0 версии в 5,8 версию и обновление пакетов. Тильда ~ обновляет до высшей патч версии. Домик ^ до высшей минорной версии. https://habr.com/ru/post/258891/

```
    "require": {
        "php": "~7.3.28",
        ...
        "laravel/framework": "6.0.*",
        ...
        "laravel/ui": "~1.0"
    },
    "require-dev": {
        "barryvdh/laravel-debugbar": "~3.0",
        ...
        "nunomaduro/collision": "~3.0",
        "phpunit/phpunit": "~8.0"
    },

```


    composer update
    или
    php composer.phar update

Поменять строчки 115-119 в файле vendor\laravel\framework\src\Illuminate\Foundation\PackageManifest.php. https://stackoverflow.com/questions/61177995/laravel-packagemanifest-php-undefined-index-name .  
Так уже изменено в новых версиях c "^6.0" при установке.

        if ($this->files->exists($path = $this->vendorPath.'/composer/installed.json')) {
            //$packages = json_decode($this->files->get($path), true);
            $installed = json_decode($this->files->get($path), true);
            $packages = $installed['packages'] ?? $installed;
        }

    composer dump-autoload

    php artisan -V

Удаление устаревших методов для blade-файлов. Artisan command to avoid Blade errors related to the removal of Lang::transChoice, Lang::trans, and Lang::getFromJson.  
https://stackoverflow.com/questions/58162258/method-illuminate-translation-translatorgetfromjson-does-not-exist

    php artisan view:clear

Протестировать сайт.  
(Можно изменить Controllers\Blog\Admin\CategoryController).


Создать новую таблицу failed_jobs (автоматически появляется начиная с версии 6.0):

    php artisan queue:failed-table

Для минимума достаточно. Но нужно обновить БД.

    php artisan migrate:refresh --seed

---

Обновление системы авторизации.  
При этом удалить некоторые маршруты в routes\web.php, т.к. они уже созданы.   

    composer require laravel/ui "~1.0"
    или
    php composer.phar require laravel/ui "~1.0"

    php artisan ui vue --auth (6.0)
    (php artisan ui:auth (8.0))

При выполнении "php artisan ui vue --auth" подтвердить замену файлов в resources\views\auth или скопировать папку resources\views\auth из 6.0 версии в 5.8.

Необязательно:  
скопировать из 6.0 версии в рабочую public\css\app.css и public\js\app.js

Установка новой таблицы, рефреш БД.

    php artisan migrate:refresh --seed

    php artisan -V

---
## 52. Подготовка очередей

Создаем миграцию для новой таблицы jobs

    php artisan queue:table

Создаем миграцию для таблицы failed_jobs, если не создавалась

    php artisan queue:failed-table

 Произвести все незавершенные миграции. Все предыдущие выполненные миграции не будут запущены. 

    php artisan migrate

Создаем первые jobs

    php artisan make:job BlogPostAfterCreateJob
    php artisan make:job BlogPostAfterDeleteJob
    php artisan make:job ProcessVideoJob

Логи здесь:

    storage\logs\

Настройки очередей:

    config\queue.php

В .env файле настройка

QUEUE_CONNECTION=sync (выполнение сразу) или =database (нужны таблицы jobs и failed_jobs)

---
## 53. Работа с очередями

Когда в таблице jobs будет создан объект Json, его параметры можно (для удобства) просмотреть на https://jsoneditoronline.org, где:
    // maxTries - количество попыток  
    // delay - отсрочка  выполнения  
    // timeout - разрешенное время процесса выполнения задачи  
    // timeoutAt - дата, до который будут попытки  


Команды "php artisan queue:" на публичном сервере должны запускаться после настройки Супервизора сервера (настройки описаны в документации Laravel).  
На локальном сервере дополнительных настроек не требуется.

    php artisan queue:work

Наиболее оптимальная для продакшн разработки.
Запускает процесс обработки задач очереди как демон.
Все изменения, сделанные в коде после запуска приняты не будут.
То есть после апдейта кода потребуется перезапуск команды.

    php artisan queue:work --queue=queueName1,queueName2

Сначала выполнятся все задачи из очереди queueName1, потом из queueName2.

    php artisan queue:listen

Наиболее оптимальная для локальной разработки.
Постоянно слушает процессы, команда не завершается самостоятельно, висит для прослушивания.
Запускает процесс обработки задач указанной очереди.
Изменения сделанные в коде после запуска будут приняты.
Хуже по производительности в сравнении с "queue:work" .
Работает с очередью по имени default, прописанному в таблице jobs.
В случае другого имени дописывается --queue=new_name:

    php artisan queue:listen --queue=new_name

    php artisan queue:restart
Мягкий перезапуск демона queue:work после того, как тот завершит выполняемую задачу.

    php artisan queue:failed

Просмотр таблицы проваленных задач.

    php artisan queue:retry all

Возврат в очередь выполнения всех проваленных задач.

    php artisan queue:retry 5

Возврат проваленной задачи (id=5) в очередь выполнения.

---
## 54. Очереди - цепочки задач

Идея - если бы это был интернет-магазин, создание одного прайса от многих точек, городов,
и предоставление его стороннему веб-сервису.

    php artisan make:job GenerateCatalog/AbstractJob

    php artisan make:job GenerateCatalog/GenerateCatalogMainJob
    php artisan make:job GenerateCatalog/GenerateCatalogCacheJob

    php artisan make:job GenerateCatalog/GeneratePricesFileChunkJobs
    php artisan make:job GenerateCatalog/GenerateCategoriesJob
    php artisan make:job GenerateCatalog/GenerateDeliveriesJob

    php artisan make:job GenerateCatalog/GeneratePointsJob
    php artisan make:job GenerateCatalog/ArchiveUploadsJobs
    php artisan make:job GenerateCatalog/SendPriceRequestJob

    php artisan make:job GenerateCatalog/GenerateGoodsFileJob





