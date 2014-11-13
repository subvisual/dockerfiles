FROM naps62/nginx:latest
MAINTAINER naps62 <mpalhas@gmail.com>

# enable nginx runit services
RUN rm -f /etc/service/nginx/down
RUN rm -f /etc/service/nginx-log-forwarder/down

ADD test-app.nginx.conf /etc/nginx/sites-enabled/default

# rbenv dependencies
RUN add-apt-repository -y "deb http://archive.ubuntu.com/ubuntu $(lsb_release -sc) universe"
RUN apt-get -y update
RUN apt-get install -y build-essential curl git
RUN apt-get install -y zlib1g-dev libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt-dev

# rbenv
RUN git clone https://github.com/sstephenson/rbenv.git /root/.rbenv
RUN git clone https://github.com/sstephenson/ruby-build.git /root/.rbenv/plugins/ruby-build
RUN ./root/.rbenv/plugins/ruby-build/install.sh
ENV PATH /root/.rbenv/bin:/root/.rbenv/shims:$PATH
RUN echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh
RUN echo 'eval "$(rbenv init -)"' >> .bashrc

# ruby
RUN echo 'gem: --no-ri --no-rdoc' >> /.gemrc
RUN rbenv install 2.1.3
RUN rbenv global  2.1.3
RUN gem install bundler rubygems-bundler

# node.js
RUN apt-get install -y nodejs npm

# pre bundle
WORKDIR /tmp
ADD test-app/Gemfile /tmp/
ADD test-app/Gemfile.lock /tmp/
RUN bundle install

# final cleanup
# as specified here: https://github.com/phusion/baseimage-docker#using
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# add rails app
ENV PORT 3000
ENV RAILS_ENV production
ADD test-app /var/www/test-app

WORKDIR /var/www/test-app
CMD ["bundle", "exec", "foreman", "start"]
