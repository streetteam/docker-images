FROM circleci-base

USER root

# Install mailcatcher, redis and rabbit, then clean up after ourselves
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential ruby ruby-dev unzip \
    redis-server rabbitmq-server ca-certificates jq && \
    gem install mailcatcher -v 0.6.5 --no-ri --no-rdoc &&  \
    apt-get remove -y --purge ruby-dev && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/* && \
    # Install terraform
    git clone https://github.com/tfutils/tfenv.git /usr/src/.tfenv && \
    ln -s /usr/src/.tfenv/bin/* /usr/local/bin && \
    tfenv install 0.14.4 && \
    tfenv use 0.14.4

# Ensure python3.6 is default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.7 10 && \
    update-alternatives --install /usr/bin/python python /usr/local/bin/python3.7 10

USER ciuser
