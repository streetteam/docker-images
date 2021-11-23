FROM ubuntu:xenial

# Install python3.8 and official PostgreSQL repo,
# add all our standard system dependencies, and then clean up after ourselves
RUN apt-get update &&  \
    apt-get install -y --no-install-recommends apt-utils wget software-properties-common curl apt-transport-https \
    build-essential checkinstall make gcc sudo curl unzip zip libglu1-mesa libxi6 gnupg-agent redis-tools jq libmemcached-dev && \
    # install gdal
    add-apt-repository ppa:ubuntugis/ppa && \
    apt-get update && \
    apt-get install -y gdal-bin libgdal-dev && \

     # install python3.8
    apt-get update && \
    apt-get install -y zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libsqlite3-dev \
    libreadline-dev libffi-dev libbz2-dev wget && \
    # dependency needed for pycurl
    apt-get install -y libgnutls28-dev && \
    cd /opt && wget https://www.python.org/ftp/python/3.8.12/Python-3.8.12.tgz && \
    tar xzf Python-3.8.12.tgz && \
    cd Python-3.8.12 && ./configure --enable-optimizations && \
    make altinstall && \

    # install pip
    curl https://bootstrap.pypa.io/get-pip.py | sudo python3.8 && \

    # airflow related deps
    apt-get install -y --no-install-recommends librabbitmq-dev libtool-bin pkg-config autoconf automake && \

    # postgres deps
    add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"  && \
    curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
    apt-get update && apt-get install -y --no-install-recommends \
    postgresql-client-12 git openssh-client ca-certificates parallel && \

    # Install docker
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable" && \
    apt-get update && apt-get install -y --no-install-recommends docker-ce docker-ce-cli containerd.io && \

    apt-get remove -y --purge software-properties-common && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*

# Ensure python3.8 is default
RUN update-alternatives --install /usr/bin/python python /usr/local/bin/python3.8 1 && \
    update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.8 1

# Install nave for NodeJS versions
RUN curl -o /usr/bin/nave https://raw.githubusercontent.com/isaacs/nave/master/nave.sh && \
    chmod +x /usr/bin/nave

# Install yarn installer, so we have it available
RUN curl -o /usr/bin/install-yarn https://yarnpkg.com/install.sh && \
    chmod +x /usr/bin/install-yarn

# Install aws cli session manager
RUN curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" \
    -o "session-manager-plugin.deb" && dpkg -i session-manager-plugin.deb

# Restrict further actions to the non-root 'ciuser' user, but with access to sudo
RUN useradd -ms /bin/bash ciuser && echo "ciuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Set correct locale and ensure locale is available in any interactive/non-interactive shell
COPY locale /etc/default/locale
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8

USER ciuser
WORKDIR /home/ciuser

# Preload NodeJS 14.15.4 LTS
RUN nave use 14.15.4 node --version

COPY bashrc .docker.bashrc
RUN cat .docker.bashrc >> .bashrc && rm .docker.bashrc
