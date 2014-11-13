# nginx docker image

This image sets up nginx on top of phusion/baseimage

## What's included:

* nginx is installed and setup via [runit](http://smarden.org/runit/), but disabled by default
* nginx-log-forwarder to forward nginx error log to docker logs
* permissions set up to receive apps to `/var/www`
* ports 80 and 443 exposed

Dockerfile:

    FROM naps62/nginx:latest

    # install app
    ADD test-app.conf /etc/nginx/sites-enabled/default
    ADD ./test-app /var/www/test-app

app.conf:

    server {
      listen 80;
      listen [::]:80;

      root /var/www/test-app;
      index index.html;

      location / {
        try_files $uri $uri/ =404;
      }

    }
