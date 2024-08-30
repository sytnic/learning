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

## 03

