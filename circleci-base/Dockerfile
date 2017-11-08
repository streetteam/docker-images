FROM ubuntu:latest

RUN apt update
RUN apt install -y software-properties-common
RUN add-apt-repository --update ppa:jonathonf/python-3.6
RUN apt install -y python3.6 libpython3.6-dev python2.7 libxml2-dev libxslt1-dev gettext libpng16-16
RUN update-alternatives --install /usr/bin/python2 python3 /usr/bin/python3.6 10
RUN ln -sf /usr/bin/python2.6 /usr/local/bin/python3.6
RUN curl -o /tmp/google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt install -y /tmp/google-chrome-stable_current_amd64.deb
