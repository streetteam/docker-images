FROM python-psql

RUN apt-get update
RUN apt-get install -y

# Add unstable repo to allow us to access latest GDAL builds
RUN echo deb http://ftp.uk.debian.org/debian stable main contrib non-free >> /etc/apt/sources.list
RUN apt-get update

## Update privs to allow Postgres to run locally
RUN sed -i "s/local   all             postgres                                peer/local   all             postgres                                trust/" /etc/postgresql/12/main/pg_hba.conf

## Existing binutils causes a dependency conflict, correct version will be installed when GDAL gets intalled
RUN apt-get remove -y binutils

RUN apt-get update && apt-get install -y gdal-bin libgdal-dev libmemcached-dev libgnutls28-dev g++

## Update C env vars so compiler can find gdal
ENV CPLUS_INCLUDE_PATH=/usr/include/gdal
ENV C_INCLUDE_PATH=/usr/include/gdal

# Install S6 and Nginx
ADD https://github.com/just-containers/s6-overlay/releases/download/v2.2.0.1/s6-overlay-amd64-installer /tmp/
RUN chmod +x /tmp/s6-overlay-amd64-installer && /tmp/s6-overlay-amd64-installer /
RUN apt-get update && apt-get install -y nginx ruby ruby-dev git

RUN apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*

WORKDIR /code
