FROM ubuntu:xenial

# Install python3.7 and official PostgreSQL repo,
# add all our standard system dependencies, and then clean up after ourselves
RUN apt-get update &&  \
    apt-get install -y --no-install-recommends apt-utils wget software-properties-common curl apt-transport-https \
    build-essential checkinstall make gcc sudo unzip && \
    # install gdal
    wget http://download.osgeo.org/gdal/2.4.2/gdal-2.4.2.tar.gz -P /usr/src && \
    tar xzf /usr/src/gdal-2.4.2.tar.gz -C /usr/src/ && \
    (cd /usr/src/gdal-2.4.2; ./configure --prefix=/usr/) && \
    make install -C /usr/src/gdal-2.4.2 && \
    # python3.7 related dependencies
    apt-get install -y --no-install-recommends python2.7-minimal libreadline-gplv2-dev \
    libncursesw5-dev libssl-dev sqlite3 libsqlite3-dev tk-dev libgdbm-dev libc6-dev \
    libbz2-dev libffi-dev zlib1g-dev gettext libgettextpo-dev && \
    # download python3.7 and install
    wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz -P /usr/src && \
    tar xzf /usr/src/Python-3.7.3.tgz -C /usr/src/ && \
    /usr/src/Python-3.7.3/configure --enable-optimizations && \
    make altinstall && \
    # install pip
    # Removing lsb_release as it causes problem when installing pip
    # see: https://github.com/pypa/pip/issues/4924
    rm -rf /usr/bin/lsb_release 2>dev/null && \
    python3.7 -m pip install --upgrade pip && \
    # Heroku deps
    curl -L https://cli-assets.heroku.com/apt/release.key | apt-key add - && \
    add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./" && \
    add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"  && \
    curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
    apt-get update && apt-get install -y --no-install-recommends \
    postgresql-client-9.6 git openssh-client heroku && \
    apt-get remove -y --purge software-properties-common && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*


# Ensure python3.7 is default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.7 10 && \
    update-alternatives --install /usr/bin/python python /usr/local/bin/python3.7 10


# Install nave for NodeJS versions
RUN curl -o /usr/bin/nave https://raw.githubusercontent.com/isaacs/nave/master/nave.sh && \
    chmod +x /usr/bin/nave

# Install yarn installer, so we have it available
RUN curl -o /usr/bin/install-yarn https://yarnpkg.com/install.sh && \
    chmod +x /usr/bin/install-yarn

# Restrict further actions to the non-root 'ciuser' user, but with access to sudo
RUN useradd -ms /bin/bash ciuser && echo "ciuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Set correct locale and ensure locale is available in any interactive/non-interactive shell
COPY locale /etc/default/locale
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8

USER ciuser
WORKDIR /home/ciuser

# Preload NodeJS 12.6.0
RUN nave use 12.6.0 node --version

COPY bashrc .docker.bashrc
RUN cat .docker.bashrc >> .bashrc && rm .docker.bashrc
