FROM bitnami/minideb:unstable

RUN apt-get update && apt-get install -y --no-install-recommends \
        apt-utils software-properties-common curl gnupg2 apt-transport-https unzip zip jq && \
    curl -L https://cli-assets.heroku.com/apt/release.key | apt-key add - && \
    add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./" && \
    apt-get update && apt-get install -y --no-install-recommends git openssh-client build-essential checkinstall \
    postgresql-client make gcc sudo && \
    # install python3.8
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.8 python3.8-dev python3.8-venv && \

    # Install terraform
    git clone https://github.com/tfutils/tfenv.git /usr/src/.tfenv && \
    ln -s /usr/src/.tfenv/bin/* /usr/local/bin && \
    tfenv install 1.0.9 && \
    tfenv use 1.0.9 && \


    # install pip
    curl https://bootstrap.pypa.io/get-pip.py | sudo python3.8 && \
    
    apt-get remove -y --purge software-properties-common gnupg2 apt-transport-https && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*

# Ensure python3.8 is default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1 && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1
    
RUN pip install --upgrade pip requests setuptools

# Install nave for NodeJS versions
RUN curl -o /usr/bin/nave https://raw.githubusercontent.com/isaacs/nave/master/nave.sh && \
    chmod +x /usr/bin/nave

# Install Sentry-cli, for FE sourcemap upload
RUN curl -sL https://sentry.io/get-cli/ | bash
    
# Restrict further actions to the non-root 'ciuser' user, but with access to sudo
RUN useradd -ms /bin/bash ciuser && echo "ciuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Install aws cli session manager
RUN curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" \
    -o "session-manager-plugin.deb" && dpkg -i session-manager-plugin.deb

# Set correct locale and ensure locale is available in any interactive/non-interactive shell
COPY locale /etc/default/locale
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8

USER ciuser
WORKDIR /home/ciuser

# heroku plugins installation
RUN heroku plugins:install heroku-repo && \
    heroku plugins:install heroku-releases-retry
