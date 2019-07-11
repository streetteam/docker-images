FROM bitnami/minideb:unstable

RUN apt-get update && apt-get install -y --no-install-recommends \
        apt-utils software-properties-common curl gnupg2 apt-transport-https unzip && \
    curl -L https://cli-assets.heroku.com/apt/release.key | apt-key add - && \
    add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./" && \
    apt-get update && apt-get install -y --no-install-recommends git openssh-client build-essential checkinstall \
    heroku make gcc sudo && \
    # python3.7 related dependencies
    apt-get install -y --no-install-recommends python2.7-minimal libreadline-gplv2-dev \
    libncursesw5-dev libssl-dev sqlite3 libsqlite3-dev tk-dev libgdbm-dev libc6-dev \
    libbz2-dev libffi-dev zlib1g-dev wget gettext libgettextpo-dev && \
    # download python3.7 and install
    wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz -P /usr/src && \
    tar xzf /usr/src/Python-3.7.3.tgz -C /usr/src/ && \
    /usr/src/Python-3.7.3/configure --enable-optimizations && \
    make altinstall && \

    # Install terraform
    wget https://releases.hashicorp.com/terraform/0.12.3/terraform_0.12.3_linux_amd64.zip -P /usr/src && \
    unzip /usr/src/terraform_0.12.3_linux_amd64.zip -d /usr/src && \
    mv /usr/src/terraform /usr/local/bin && \

    # install pip
    # Removing lsb_release as it causes problem when installing pip
    # see: https://github.com/pypa/pip/issues/4924
    rm -rf /usr/bin/lsb_release 2>dev/null && \
    python3.7 -m pip install --upgrade pip && \
    apt-get remove -y --purge software-properties-common curl gnupg2 apt-transport-https && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*

# Ensure python3.7 is default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.7 10 && \
    update-alternatives --install /usr/bin/python python /usr/local/bin/python3.7 10
    
RUN pip install --upgrade pip requests setuptools

# Restrict further actions to the non-root 'ciuser' user, but with access to sudo
RUN useradd -ms /bin/bash ciuser && echo "ciuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Set correct locale and ensure locale is available in any interactive/non-interactive shell
COPY locale /etc/default/locale
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8

USER ciuser
WORKDIR /home/ciuser

COPY bashrc .docker.bashrc
RUN cat .docker.bashrc >> .bashrc && rm .docker.bashrc
