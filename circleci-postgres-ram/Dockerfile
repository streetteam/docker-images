FROM cimg/postgres:12.8-postgis


# increase PG connections
RUN if [ -e /usr/local/share/postgresql/postgresql.conf.sample ]; then \
        postgresfile=/usr/local/share/postgresql/postgresql.conf.sample; \
    else \
        postgresfile=/usr/share/postgresql/postgresql.conf.sample; \
    fi && \
    echo max_connections=200 >> $postgresfile &&\
    echo max_locks_per_transaction=128 >> $postgresfile
