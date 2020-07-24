FROM circleci-fullstack

USER root

 # from https://github.com/cypress-io/cypress-docker-images/blob/master/base/8/Dockerfile
RUN apt-get update && \
  apt-get install -y \
    libgtk-3-0 \
    fonts-liberation \
    libappindicator3-1 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libatspi2.0-0 \
    libcairo2 \
    libcups2 \
    libdbus-1-3 \
    libgbm1 \
    libgdk-pixbuf2.0-0 \
    libglib2.0-0 \
    libnspr4 \
    libpango-1.0-0 \
    libpangocairo-1.0-0 \
    libxcomposite1 \
    libxcursor1 \
    libxi6 \
    libxrandr2 \
    libxrender1 \
    libxtst6 \
    xdg-utils \
    libnotify-dev \
    libgconf-2-4 \
    libnss3 \
    libxss1 \
    libasound2 \
    xvfb

# download chrome
RUN curl --silent --show-error --location --fail --retry 3 \
         --output google-chrome-stable_current_amd64.deb \
         https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
  dpkg -i google-chrome-stable_current_amd64.deb && \
  rm -rf google-chrome-stable_current_amd64.deb

# Ensure python3.7 is default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.7 10 && \
    update-alternatives --install /usr/bin/python python /usr/local/bin/python3.7 10

USER ciuser
