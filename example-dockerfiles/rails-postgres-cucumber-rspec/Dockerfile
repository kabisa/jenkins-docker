FROM ruby:2.2.1

RUN apt-get update && apt-get -y install \
    build-essential \
    git-core \
    curl libssl-dev \
    libreadline-dev \
    zlib1g zlib1g-dev \
    libcurl4-openssl-dev \
    libxslt-dev libxml2-dev \
    xvfb nodejs-legacy \
    postgresql \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV CONTAINER_INIT /usr/local/bin/init-container
RUN echo '#!/usr/bin/env bash' > $CONTAINER_INIT ; chmod +x $CONTAINER_INIT

RUN sed -i 's/md5\|peer/trust/' /etc/postgresql/*/main/pg_hba.conf
RUN echo 'service postgresql start' >> $CONTAINER_INIT

RUN gem install bundler
RUN bundle config --global path /cache/
RUN echo 'bundle config --global jobs $(cat /proc/cpuinfo | grep -c processor)' >> $CONTAINER_INIT
RUN gem install rubygems-update && update_rubygems
ENV BUNDLE_GEMFILE /workspace/Gemfile

RUN echo 'Xvfb :0 -ac -screen 0 1024x768x24 >/dev/null 2>&1 &' >> $CONTAINER_INIT
