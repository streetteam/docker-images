FROM circleci-base

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    software-properties-common gnupg2 jq

# Install terraform
RUN git clone https://github.com/tfutils/tfenv.git /usr/src/.tfenv && \
    ln -s /usr/src/.tfenv/bin/* /usr/local/bin && \
    tfenv install 1.0.9 && \
    tfenv use 1.0.9

# Clean up
RUN apt-get remove -y --purge software-properties-common && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*


RUN pip install --upgrade pip requests setuptools

# Install Sentry-cli, for FE sourcemap upload
RUN curl -sL https://sentry.io/get-cli/ | bash

# Install aws cli session manager
RUN curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" \
    -o "session-manager-plugin.deb" && dpkg -i session-manager-plugin.deb

USER ciuser
