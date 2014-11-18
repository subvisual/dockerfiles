FROM naps62/rails:latest

RUN rbenv install 2.1.3
RUN rbenv global 2.1.3
RUN gem install bundler

WORKDIR /tmp
ADD Gemfile /tmp/
ADD Gemfile.lock /tmp/
RUN bundle install

RUN rm -rf /tmp/*

ENV RAILS_ENV production
ENV APP_NAME test-app
WORKDIR /var/www/test-app

CMD ["bundle", "exec", "foreman", "start"]

ADD config/nginx.conf /etc/nginx/sites-enabled/test-app.conf
ADD . /var/www/test-app
RUN bundle exec rake assets:precompile
