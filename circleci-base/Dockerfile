FROM ubuntu:xenial

# Trusted PPA for Python 3.6.x releases on xenial and official PostgreSQL repo,
# add all our standard system dependencies, and then clean up after ourselves
RUN apt-get update &&  \
    apt-get install -y --no-install-recommends software-properties-common curl apt-transport-https && \
    add-apt-repository ppa:jonathonf/python-3.6 && \
    curl -L https://cli-assets.heroku.com/apt/release.key | apt-key add - && \
    add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./" && \
    add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"  && \
    curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
    apt-get update && apt-get install -y --no-install-recommends \
    libpython3.6-dev python2.7-minimal libxml2-dev libxslt1-dev gettext zlib1g-dev libjpeg-dev \
    libpng12-0 postgresql-client-9.6 git openssh-client heroku \
    python3.6 python3.6-venv libpython3.6 libpython3.6-stdlib python3.6-dev make \
    gcc sudo && \
    apt-get remove -y --purge software-properties-common && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*


# Ensure python3.6 is default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 10 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.6 10
 
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

# Preload NodeJS 8.5.0
RUN nave use 8.5.0 node --version

COPY bashrc .docker.bashrc
RUN cat .docker.bashrc >> .bashrc && rm .docker.bashrc
