FROM ubuntu:xenial

RUN apt-get update
RUN apt-get upgrade -y

# Trusted PPA for Python 3.6.x releases on xenial
RUN apt-get install -y software-properties-common && add-apt-repository --update ppa:jonathonf/python-3.6

# Add all our standard system dependencies
RUN apt-get install -y python3.6 python3.6-venv python3.6-dev \
                       libpython3.6-dev python2.7 libxml2-dev libxslt1-dev gettext \
		       libpng16-16 postgresql-client git openssh-client \
		       make gcc locales

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

# Restrict further actions to the non-root 'ciuser' user
RUN useradd -ms /bin/bash ciuser
USER ciuser
WORKDIR /home/ciuser

# Preload NodeJS 8.5.0
RUN nave use 8.5.0 node --version

# Ensure locale is available in any interactive/non-interactive shell
RUN cat /etc/default/locale >> /home/ciuser/.bashrc
