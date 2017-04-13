# Nginx frontend-server for Docker-based applications

Текущая конфигурация может пригодится тем, кому нужно запускать за frontend (edge)
сервером несколько приложений внутри docker-контейнеров каждый со своим веб-сервером.

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

## Placeholder

Placeholder HTML-page: /placeholder/index.html