## 01.Add.a.post

Посты создаются в папке _posts. Их можно создавать вручную, но лучше установить плагин для автоматического создания файла поста:  

> Gemfile

```
...
gem 'jekyll-compose', group: [:jekyll_plugins]

```

Терминал. Чтоб изменения вступили в силу.    

    bundle

Теперь можно создать файл поста из терминала:  

    bundle exec jekyll post "Hello world"  

И заполнить его текстом в формате маркдаун.  

Туториал по маркдауну:  

https://www.markdowntutorial.com/

## 02.Edit.front.matter.content

https://jekyllrb.com/docs/front-matter/

Во front matter можно прописывать свои категории и тэги.  

## 03.Add.a.page

Создание страницы из терминала

    bundle jekyll page "Projects"

## 04.Add.static.content

Дополнительные папки и файлы, помещенные в корень проекта, будут доступны движку jekyll и могут быть использованы для создания сайта. Например, это могут быть стили или изображения для добавления на страницы. 

## 05.Customize.the.error.page

Страница может быть настроена вручную. Она лежит в корне проекта jekyll.

## 06.Generate.the.site.files

Генерация файлов в папку _site (public) для выкладывания на хостинг.  
    
    bundle exec jekyll build

## 07.Learn.more.about.Jekyll

Основы заложены. Далее будет изучаться выкладка сайта в интернет. Более подробно Jekyll изучается на курсе "Jekyll for Web Designers" by James Williamson.  

---

