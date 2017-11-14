FROM ubuntu:xenial

# Trusted PPA for Python 3.6.x releases on xenial and official PostgreSQL repo,
# add all our standard system dependencies, and then clean up after ourselves
RUN apt-get update &&  \
    apt-get install -y --no-install-recommends software-properties-common curl && \
    add-apt-repository ppa:jonathonf/python-3.6 && \
    add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"  && \
    curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
    apt-get update && apt-get install -y --no-install-recommends \
    libpython3.6-dev python2.7-minimal libxml2-dev libxslt1-dev gettext \
    libpng16-16 postgresql-client-9.6 git openssh-client \
    locales build-essential libsqlite3-dev ruby-dev \
    python3.6 python3.6-venv python3.6-dev \
    redis-server rabbitmq-server make gcc \
    xvfb sudo && \
    curl -o /tmp/google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
    apt install -y --no-install-recommends /tmp/google-chrome-stable_current_amd64.deb && \
    rm /tmp/google-chrome-stable_current_amd64.deb && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/*

# Make Python 3.6.x the default python3
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 10 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.6 10

# Setup Xvfb
ENV DISPLAY :99
COPY xvfb.init /etc/init.d/xvfb
RUN echo DISPLAY=":99" >> /etc/environment && chmod +x /etc/init.d/xvfb
    
# Install nave for NodeJS versions
RUN curl -o /usr/bin/nave https://raw.githubusercontent.com/isaacs/nave/master/nave.sh && \
    chmod +x /usr/bin/nave

# Install yarn installer, so we have it available
RUN curl -o /usr/bin/install-yarn https://yarnpkg.com/install.sh && \
    chmod +x /usr/bin/install-yarn

# Install mailcatcher
RUN gem install mailcatcher --no-ri --no-rdoc

# Restrict further actions to the non-root 'ciuser' user, but with access to sudo
RUN useradd -ms /bin/bash ciuser && echo "ciuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Set correct locale and ensure locale is available in any interactive/non-interactive shell
RUN locale-gen en_GB.utf8 && update-locale LC_ALL=en_GB.utf8 LANG=en_GB.utf8 LANGUAGE=en_GB.utf8 && \
    cat /etc/default/locale >> /home/ciuser/.bashrc
ENV LC_ALL=en_GB.utf8
ENV LANG=en_GB.utf8
ENV LANGUAGE=en_GB.utf8

USER ciuser
WORKDIR /home/ciuser

# Preload NodeJS 8.5.0
RUN nave use 8.5.0 node --version
