## 006-The Linux file system

Папки для хранения программ:  

bin  
sbin  
usr 

Для монтирования дисков:  

media  
mnt  

Папка файлов конфигурации:  

etc  

Папка с изменяемой системной инфромацией:  

var  

Системные и другие логи:  

var/log  

Каталоги созданные ядром, они представляют аппаратное обеспечение, включая системное оборудование, процессы, настройки ядра:

dev  
proc  
sys 

## 008-Choosing a Linux distribution

Команда для описания дистрибутива

    cat /etc/*release

## 011-Helping

В терминале:

    man (имя команды)
    // example:
    man ls
    // навигация:
    F
    B
    // выход:
    Q

В интернете:

    man.he.net
    manpages.net

Форумы:

    unix.stackexchange.com, serverfault.com

Узнать версию ядра

    uname -a