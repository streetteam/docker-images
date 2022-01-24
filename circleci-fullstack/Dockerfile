FROM circleci-base

USER root

# Install mailcatcher, redis and rabbit, then clean up after ourselves
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential ruby ruby-dev unzip \
    redis-server rabbitmq-server ca-certificates jq && \
    # Note: `--no-ri` and `--no-rdoc` is now deprecated in ubuntu focal, the option is now `--no-document`
    gem install mailcatcher -v 0.6.5 --no-document &&  \
    apt-get remove -y --purge ruby-dev && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/* && \
    # Install terraform

RUN git clone https://github.com/tfutils/tfenv.git /usr/src/.tfenv && \
    ln -s /usr/src/.tfenv/bin/* /usr/local/bin && \
    tfenv install 1.0.9 && \
    tfenv use 1.0.9

# Ensure python3.8 is default
RUN update-alternatives --install /usr/bin/python python /usr/local/bin/python3.9 1 && \
    update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.9 1

USER ciuser
