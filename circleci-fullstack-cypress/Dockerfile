FROM circleci-fullstack

USER root

 # from https://github.com/cypress-io/cypress-docker-images/blob/master/base/8/Dockerfile
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    libgtk2.0-0 \
    libnotify-dev \
    libgconf-2-4 \
    libnss3 \
    libxss1 \
    libasound2 \
    xvfb

USER ciuser
