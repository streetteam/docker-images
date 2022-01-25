FROM circleci-fullstack

USER root

 # from https://docs.cypress.io/guides/continuous-integration/introduction#Dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    libgtk2.0-0 libgtk-3-0 libgbm-dev libnotify-dev libgconf-2-4 libnss3 libxss1 libasound2 libxtst6 xauth xvfb


USER ciuser
