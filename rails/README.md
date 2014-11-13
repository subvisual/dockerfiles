# Rails docker image

This image uses nginx and foreman to deploy a rails app

## What's included:

* nginx is already installed and enabled as a runit service
* rbenv is installed (but without rubies)
* nodejs
* .gemrc with "--no-ri --no-rdoc" options, for faster gem installations
* `bundle exec foreman start` as the default run command

## Example usage

Dockerfile:

    FROM naps62/rails:latest

    # rbenv is installed without any rubies
    # install one and set it as default
    RUN rbenv install 2.1.3
    RUN rbenv global 2.1.3
    RUN gem install bundler

    # pre bundle
    # this is optional, but will speed up builds, so that bundle install will be
    # skipped when the Gemfile is unchaged
    WORKDIR /tmp
    ADD Gemfile      /tmp/
    ADD Gemfile.lock /tmp/
    RUN bundle install

    # clean up
    RUN rm -rf /tmp/*

    # install your app
    ADD . /var/www/my-app
    ADD config/nginx.conf /etc/nginx/sites-enabled/my-app
    ENV RAILS_ENV productionn

    WORKDIR /var/www/my-app


Procfile

    web: bundle exec puma -C config/puma.rb


config/nginx.conf:

    server {
      listen 80;

      root /var/www/my-app;

      location / {
      }
    }
