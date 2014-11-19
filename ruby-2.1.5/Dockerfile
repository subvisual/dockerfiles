FROM groupbuddies/rbenv:latest
MAINTAINER naps62 <mpalhas@groupbuddies.com>

RUN rbenv install 2.1.3
RUN rbenv global 2.1.3
RUN gem install bundler

# pre bundle
ONBUILD ADD Gemfile /tmp/
ONBUILD ADD Gemfile.lock /tmp/
ONBUILD RUN cd /tmp && bundle install

RUN rm -rf /tmp/*

# install
WORKDIR /var/www/app
CMD ["bundle", "exec", "foreman", "start"]
