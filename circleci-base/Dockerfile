FROM ubuntu:focal

# Install system essentials
RUN apt-get update && \
    apt-get install -y --no-install-recommends software-properties-common && \
    apt-get install -y --no-install-recommends git openssh-client ca-certificates parallel curl gnupg lsb-release

# Install GDAL
RUN add-apt-repository ppa:ubuntugis/ppa && \
    apt-get update && \
    apt-get install -y --no-install-recommends libgdal-dev libgnutls28-dev libmemcached-dev postgresql-client gettext

# install python3.9
RUN add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.9 python3-pip

# Airflow related deps
RUN apt-get install -y --no-install-recommends librabbitmq-dev libtool-bin pkg-config autoconf automake

# Install postgres deps
RUN apt-get install -y --no-install-recommends postgresql-client-12

# Install docker
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
     echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
      apt-get update && apt-get install -y --no-install-recommends docker-ce docker-ce-cli containerd.io

# Clean up
RUN apt-get remove -y --purge software-properties-common && \
    apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*


# Ensure python3.9 is default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1 && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1

# Install nave for NodeJS versions
RUN curl -o /usr/bin/nave https://raw.githubusercontent.com/isaacs/nave/master/nave.sh && \
    chmod +x /usr/bin/nave


# Install aws cli session manager
RUN curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" \
    -o "session-manager-plugin.deb" && dpkg -i session-manager-plugin.deb

# Restrict further actions to the non-root 'ciuser' user, but with access to sudo
RUN useradd -ms /bin/bash ciuser && echo "ciuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Set correct locale and ensure locale is available in any interactive/non-interactive shell
COPY locale /etc/default/locale
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8

USER ciuser
WORKDIR /home/ciuser

# Preload NodeJS 14.15.4 LTS
RUN nave use 14.15.4 node --version

COPY bashrc .docker.bashrc
RUN cat .docker.bashrc >> .bashrc && rm .docker.bashrc
