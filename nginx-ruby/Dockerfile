FROM phusion/baseimage:0.9.15
MAINTAINER naps62 <mpalhas@gmail.com>

# steps required for phusino/baseimage setup
# as specified here: https://github.com/phusion/baseimage-docker#using
ENV HOME /root
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh
CMD ["/sbin/my_init"]

# nginx
RUN add-apt-repository -y ppa:nginx/stable
RUN apt-get update
RUN apt-get install -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
RUN rm /etc/nginx/sites-enabled/default
RUN chown -R www-data:www-data /var/lib/nginx

# rbenv dependencies
RUN apt-get install -y -q python-software-properties
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

# final cleanup
# as specified here: https://github.com/phusion/baseimage-docker#using
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
