## 02.Install.Jekyll

Инструкция на jekyllrb.com

Сначала следует установить и проверить 3 элемента:  

    ruby -v
    gem -v
    jekyll -v

## 03.Install.Git

Для Windows:

git-for-windows.github.io  
или  
https://gitforwindows.org/

Проверка:

    git --version

## 04.Create.a.new.Jekyll.site

Создание сайта

    jekyll new awesome-static-site

Открыть текущую папку в VSCode из терминала

    code .

Создание гит-репозитория

    git init

    git add --all
    git commit -m "Create initial project"

Дополнительно для Windows

    bundle lock --add-platform ruby
    bundle lock --add-platform x86_64-linux

## 05.Preview.the.site

    bundle exec jekyll serve
    or
    jekyll serve

## 06.Install.a.theme

> Темы:

jekyllthemes.org  
https://jekyllthemes.io/

Два способа установки темы:  

### 1. Клонирование репозитория с github. Тогда не нужно использовать команду jekyll new mysite.

    git clone ...


### 2. Установка темы как дополнения к сайту через gem

Тема из  
https://broccolini.net/athena/  
https://github.com/broccolini/athena  

( Тема chirpy:  
https://chirpy.cotes.page/posts/getting-started/  
https://github.com/cotes2020/jekyll-theme-chirpy  
)

> Изменения в Gemfile:

    -- gem "minima", "~> 2.0"
    ++ gem "jekyll-athena"

> Изменения в confyg.yml:

    -- theme: minima
    ++ theme: jekyll-athena  
    
> Набрать в Windows в терминале:

    # принятие изменений
    bundle

    # запуск сервера
    bundle exec jekyll serve

## 07.The.site.configuration.file

> Изменения в confyg.yml:

новая запись:  

    destination: public

В этом случае папкой сайта с html-файлами будет public вместо _site по умолчанию.  

В .gitignore вносится запись, чтобы не выкладывать готовый html-сайт на github:  

> .gitignore

    ...
    public

---
