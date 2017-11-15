FROM circleci-fullstack

USER root

# Install deps to download and install Chrome and then clean up after ourselves
RUN apt-get update &&  \
    apt-get install -y --no-install-recommends xvfb && \
    curl -o /tmp/google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
    apt install -y --no-install-recommends /tmp/google-chrome-stable_current_amd64.deb && \
    rm /tmp/google-chrome-stable_current_amd64.deb && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*

# Ensure python3.6 is default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 10 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.6 10

# Setup Xvfb
ENV DISPLAY :99
COPY xvfb.init /etc/init.d/xvfb
RUN echo DISPLAY=":99" >> /etc/environment && chmod +x /etc/init.d/xvfb

USER ciuser
