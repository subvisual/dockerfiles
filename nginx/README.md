# Using this container

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
