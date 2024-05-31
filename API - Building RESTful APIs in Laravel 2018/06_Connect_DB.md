#

Подключиться можно и к sqlite базе данных.  
Для этого файл database.sqlite помещается в корневую папку проекта. И прописываются соответствующие значения в файле env. В Windows значения для DB_DATABASE пишутся в кавычках. 

env
```
DB_CONNECTION=sqlite
DB_HOST=127.0.0.1 
DB PORT=3306
DB_DATABASE=/Users/justin/Documents/community-poll/database.sqlite
DB_USERNAME=homestead
DB_PASSWORD=secret
```

Создание таблиц из файлов миграции:

    php artisan migrate
    or
    php artisan migrate:fresh --seed

Если миграции менялись:

    php artisan migrate:refresh

Создание моделей:

    php artisan make:model Poll
    php artisan make:model Question
    php artisan make:model Answer

Заполнение таблиц:

    php artisan db:seed

https://sqlitebrowser.org/ - DB Browser for SQLite

