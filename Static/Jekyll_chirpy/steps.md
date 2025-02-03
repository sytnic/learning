Ссылки:

https://www.youtube.com/watch?v=F8iOU1ci19Q&list=PLGpOxnoGEP2NCkQG5IXsqIZiMdhUv24Dv&index=4

https://github.com/cotes2020/jekyll-theme-chirpy

https://chirpy.cotes.page/posts/getting-started/

https://github.com/cotes2020/jekyll-theme-chirpy/wiki

По теме:

https://docs.technotim.live/posts/jekyll-docs-site/


## Обновление своей системы

    sudo apt update
    sudo apt-get install ruby-full build-essential zlib1g-dev git

Install Jekyll bundler

    sudo gem install jekyll bundler

## Создать новый публичный репозиторий онлайн. 

В него поместить chirpy. Автор выполняет это с помощью так называемого Starter, который видимо вилкует (fork) chirpy в личный новый репозиторий с одним начальным коммитом.

    

## Клон  

    git clone свой_проект_себе

## Зависимости

    cd repo-name
    bundle

## Commit and push

After making changes to your site, commit and push then up to git

    git add .
    git commit -m "made some changes"
    git push

## Running Local Server

    (sudo) bundle exec jekyll s

В сущности всё. Каркас работает.

## Building your site in production mode

    JEKYLL_ENV=production bundle exec jekyll b

This will output the production site to _site

## Building with Docker

Create a Dockerfile with the following

    FROM nginx:stable-alpine
    COPY _site /usr/share/nginx/html

Build site in production mode

    JEKYLL_ENV=production bundle exec jekyll b

Then build your image:

    docker build .
    
## Создание контента

9:24

