FROM ubuntu:xenial

RUN apt-get update
RUN apt-get upgrade -y

RUN apt-get install -y software-properties-common curl

# Trusted PPA for Python 3.6.x releases on xenial
RUN add-apt-repository ppa:jonathonf/python-3.6

# Official PostgreSQL repo
RUN add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"
RUN curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

RUN apt-get update

# Add all our standard system dependencies
RUN apt-get install -y python3.6 python3.6-venv python3.6-dev \
                       libpython3.6-dev python2.7 libxml2-dev libxslt1-dev gettext \
		       libpng16-16 postgresql-client-9.6 git openssh-client \
		       make gcc xvfb sudo \
		       locales build-essential libsqlite3-dev ruby-dev

# Set correct locale
RUN locale-gen en_GB.utf8
RUN update-locale LC_ALL=en_GB.utf8 LANG=en_GB.utf8 LANGUAGE=en_GB.utf8
ENV LC_ALL=en_GB.utf8
ENV LANG=en_GB.utf8
ENV LANGUAGE=en_GB.utf8

# Make Python 3.6.x the default python3
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 10
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.6 10

# Install latest stable Google Chrome browser
RUN curl -o /tmp/google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt install -y /tmp/google-chrome-stable_current_amd64.deb

# Install nave for NodeJS versions
RUN curl -o /tmp/nave.sh https://raw.githubusercontent.com/isaacs/nave/master/nave.sh
RUN cp /tmp/nave.sh /usr/bin/nave
RUN chmod +x /usr/bin/nave

# Install yarn installer, so we have it available
RUN curl -o /tmp/install-yarn.sh https://yarnpkg.com/install.sh
RUN cp /tmp/install-yarn.sh /usr/bin/install-yarn
RUN chmod +x /usr/bin/install-yarn

# Setup Xvfb
ENV DISPLAY :99
RUN echo DISPLAY=":99" >> /etc/environment
COPY xvfb.init /etc/init.d/xvfb
RUN chmod +x /etc/init.d/xvfb

# Install mailcatcher
RUN gem install mailcatcher --no-ri --no-rdoc

# Restrict further actions to the non-root 'ciuser' user, but with access to sudo
RUN useradd -ms /bin/bash ciuser
RUN echo "ciuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

USER ciuser
WORKDIR /home/ciuser

# Preload NodeJS 8.5.0
RUN nave use 8.5.0 node --version

# Ensure locale is available in any interactive/non-interactive shell
RUN cat /etc/default/locale >> /home/ciuser/.bashrc
