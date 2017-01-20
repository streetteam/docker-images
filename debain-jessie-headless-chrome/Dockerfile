FROM debian:jessie

RUN apt-get update && apt-get install bzip2 libfontconfig wget curl -y

RUN echo "===> install Chrome and xvfb" && \
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list && \
    apt-get update && \
    apt-get install -y google-chrome-stable

RUN echo "===> install node and npm" && \
    curl -sL https://deb.nodesource.com/setup_6.x | bash - && \
    apt-get install -y nodejs

RUN apt-get install -y xvfb && \
    apt-get clean
