FROM java:openjdk-8

RUN apt-get update \
  && apt-get -y install maven\
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV CONTAINER_INIT /usr/local/bin/init-container
RUN echo '#!/usr/bin/env bash' > $CONTAINER_INIT ; chmod +x $CONTAINER_INIT
RUN echo 'mkdir -p /cache/m2 && ln -sf /cache/m2 ~/.m2' >> $CONTAINER_INIT
