FROM ubuntu:xenial

RUN apt update
RUN apt install -y software-properties-common

# Trusted PPA for Python 3.6.x releases on xenial
RUN add-apt-repository --update ppa:jonathonf/python-3.6

# Add all our standard system dependencies
RUN apt install -y python3.6 libpython3.6-dev python2.7 libxml2-dev libxslt1-dev gettext libpng16-16

# Make Python 3.6.x the default python3
RUN update-alternatives --install /usr/bin/python2 python3 /usr/bin/python3.6 10
RUN ln -sf /usr/bin/python2.6 /usr/local/bin/python3.6

# Install latest stable Google Chrome browser
RUN curl -o /tmp/google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt install -y /tmp/google-chrome-stable_current_amd64.deb

# Install nave for NodeJS versions
RUN curl -o /tmp/nave.sh https://raw.githubusercontent.com/isaacs/nave/master/nave.sh
RUN cp /tmp/nave.sh /usr/bin/nave
RUN chmod +x /usr/bin/nave

# Restrict further actions to the non-root 'ubuntu' user
USER ubuntu
WORKDIR /home/ubuntu

# Preload NodeJS 8.5.0
RUN nave use 8.5.0 node --version
