FROM python-psql

## Update C env vars so compiler can find gdal
ENV CPLUS_INCLUDE_PATH=/usr/include/gdal
ENV C_INCLUDE_PATH=/usr/include/gdal

RUN \
    apt-get update \
    && apt-get install --no-install-recommends -y \
    && echo "deb http://ftp.uk.debian.org/debian stable main contrib non-free" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends postgresql-12-postgis-2.5 \
    ## Update privs to allow Postgres to run locally
    && sed -i "s/local   all             postgres                                peer/local   all             postgres                                trust/" /etc/postgresql/12/main/pg_hba.conf \
    ## Existing binutils causes a dependency conflict, correct version will be installed when GDAL gets installed
    && apt-get remove -y binutils \
    && apt-get update \
    && apt-get install --no-install-recommends -y gdal-bin libgdal-dev libmemcached-dev libgnutls28-dev g++ \
    # Install S6 and Nginx
    && wget --quiet -O /tmp/s6-overlay-amd64-installer "https://github.com/just-containers/s6-overlay/releases/download/v2.2.0.3/s6-overlay-amd64-installer" \
    && chmod +x /tmp/s6-overlay-amd64-installer  \
    && /tmp/s6-overlay-amd64-installer / \
    && apt-get update \
    && apt-get install -y --no-install-recommends nginx ruby ruby-dev git \
    # Clean up \
    && apt-get remove -y postgresql-12-postgis-2.5 \
    && apt-get clean -y && apt-get autoremove --purge -y && rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/* /var/cache/*

WORKDIR /code
