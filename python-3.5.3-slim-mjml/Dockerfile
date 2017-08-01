FROM python:3.5.3-slim

ENV MJML="mjml@3.2.2"

RUN apt-get update && \
    apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_6.x | bash - && \
    apt-get install -y nodejs gettext git && \
    npm install -g $MJML && \
    apt-get purge -y --auto-remove curl && \
    apt-get autoremove --purge -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
