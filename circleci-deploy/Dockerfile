FROM bitnami/minideb:unstable

RUN apt-get update && apt-get install -y --no-install-recommends \
        software-properties-common curl gnupg2 apt-transport-https && \
    curl -L https://cli-assets.heroku.com/apt/release.key | apt-key add - && \
    add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./" && \
    apt-get update && apt-get install -y --no-install-recommends git openssh-client heroku python3.6-minimal python3.6-venv python3-pip python3-setuptools python3-wheel sudo make && \
    apt-get install -y libpython3.6 libpython3.6-stdlib && \
    apt-get remove -y --purge software-properties-common curl gnupg2 apt-transport-https && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/* && \
# https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=918881
# debian unstable only ships distutils with python3.7
# this is an ugly hack to get it working for 3.6
    cp -arv /usr/lib/python3.7/distutils /usr/lib/python3.6

# Ensure python3.6 is default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 10 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.6 10 && \
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10
    
RUN pip3 install --upgrade pip requests setuptools

# Restrict further actions to the non-root 'ciuser' user, but with access to sudo
RUN useradd -ms /bin/bash ciuser && echo "ciuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Set correct locale and ensure locale is available in any interactive/non-interactive shell
COPY locale /etc/default/locale
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8

USER ciuser
WORKDIR /home/ciuser

COPY bashrc .docker.bashrc
RUN cat .docker.bashrc >> .bashrc && rm .docker.bashrc
