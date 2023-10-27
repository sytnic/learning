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
После копирования в соответствующие директории файлов Фактори, Сидер и Миграции выполнить команды для наполнения базы данных:

    php artisan migrate
    or
    php artisan migrate:fresh --seed
    php artisan migrate:refresh

    php artisan make:model Poll
    php artisan make:model Question
    php artisan make:model Answer

    php artisan db:seed

https://sqlitebrowser.org/ - DB Browser for SQLite

