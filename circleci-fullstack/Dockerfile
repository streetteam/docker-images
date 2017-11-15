FROM circleci-base

USER root

# Install mailcatcher, redis and rabbit, then clean up after ourselves
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential libsqlite3-dev ruby ruby-dev \
    redis-server rabbitmq-server && \
    gem install mailcatcher --no-ri --no-rdoc && \
    apt-get remove -y --purge build-essential ruby-dev libsqlite3-dev && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*

# Ensure python3.6 is default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 10 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.6 10


USER ciuser
