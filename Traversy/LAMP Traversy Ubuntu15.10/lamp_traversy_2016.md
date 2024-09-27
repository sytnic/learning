https://www.youtube.com/watch?v=vazRx1Ei8VA

## Commands

    sudo apt-get update

> Apache

    sudo apt-get install apache2

Уже должен работать apache в браузере с

    localhost

Должен работать доступ из сети в браузере по ip машины, содержащей apache2, если сеть между машиной и хостом настроена. В настройках сети виртуальной машины требуется - Сетевой мост (Bridged Adapter). Проверка ip в машине:

    # linux
    ifconfig

> PHP

Установка PHP. Устанавливается 7 версия по умолчанию.

    sudo apt show php
    sudo apt install php
    //или - sudo apt-get install php
    php -v

    // установка php5, если требуется
    sudo apt-get install php5

Перезапуск apache

    sudo /etc/init.d/apache2 restart

Тест работы php

    sudo nano /var/www/html/test.php

В файле ввести

    <?php echo 'It works'; ?>

Сохранить

    ctrl+x

Проверка в браузере linux'a

    localhost/test.php

> MySQL

Установка

    sudo apt-get install mysql-server

Установить пароль для root user

Проверка

    mysql -u root -p

    create database testdb;
    show databases;
    
    ctrl+C

> PHPmyAdmin

    sudo apt-get install phpmyadmin

Установить запрошенные пароли для PHPmyAdmin (MySQL, root).  

Настройка. Иначе не открывается phpmyadmin по localhost.

    sudo nano /etc/php5/apache2/php.ini

Расскоментировать (убрать ;)

    ;extension=mysql.so

Сохранить

    ctrl+x

Настройка apache config

    sudo nano /etc/apache2/apache2.conf

Внизу файла ввести

    Include /etc/phpmyadmin/apache.conf

Сохранить

    ctrl+x

Перезапуск apache

    sudo /etc/init.d/apache2 restart

Работает в браузере

    localhost/phpmyadmin/

Вход

    root
    выбранный пароль

---

