# Nginx frontend-server for Docker-based applications

## Кому будет полезно

Текущая конфигурация может пригодится тем, кому нужно запускать за frontend (edge)
сервером несколько приложений внутри docker-контейнеров каждый со своим веб-сервером.

## Как использовать

Конфигурация готова к работе сразу. При необходимости можно внести изменения
в шаблон заглушки.


```
# Качаем master-ветку на сервер
wget https://github.com/krsnv/nginx-frontend/archive/master.zip

# Распаковываем и удаляем архив
unzip master.zip && rm master.zip

# Переходим в проект
cd nginx-frontend-master # либо nginx-frontend, если вы клонировали проект

# Запускаем
docker-compose up

```

## Default host configuration

```
server {

    listen       80;
    server_name  _; # Any domain deligated to this server

    charset utf-8;

    #access_log  /var/log/nginx/log/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    
    error_page   500 502 503 504  /50x.html;

    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

## Включение/отключение доменов

Чтобы выключить домен, достаточно добавить к имени конфигурации `.disabled`.
Например:

```
example.com.conf          # Активирован
example.com.conf.disabled # Деактивирован
```

Контейнер после этого должен быть перезапущен для обновления конфигурации.

После отключения домена все запросы будут уходить на "заглушку" (placeholder)
и не будут обработаны backend'ом.

## Заглушка (Placeholder)

Шаблон страницы заглушки: /placeholder/index.html

Заглушка используется для всех доменов, которые направлены на сервер, но для них
еще нет конфигурации.

## SSL для доменов

Текущая конфигурация подразумевает, что у вас установлен Let's Encrypt и вы
монтируете его директорию с сертификатами внутрь контейнера (см. docker-compose.yml).

Обновление сертификатов происходит так, как это описано в документации
Let's Encrypt. Другие сертификаты можно также положить в директорию /ssl и прокинуть
ее внутрь контейнера.

Также доступен способ использования Let's Encrypt через дополнительный Docker-контейнер:
https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion (в работе не проверял
и буду рад, если кто-то сообщит об успешности или неуспешности метода).