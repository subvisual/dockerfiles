FROM naps62/rails:latest

RUN rbenv install 2.1.3
RUN rbenv global 2.1.3
RUN gem install bundler

WORKDIR /tmp
ADD Gemfile /tmp/
ADD Gemfile.lock /tmp/
RUN bundle install

RUN rm .rf /tmp/*

ADD . /var/www/test-app
ADD config/nginx.conf /etc/nginx/sites-enabled/test-app
ENV RAILS_ENV production

WORKDIR /var/www/test-app
