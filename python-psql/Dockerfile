FROM python:3.9.9-slim-buster

RUN \
  # Install new postgres deb repo
  apt-get update && apt-get install -y --no-install-recommends wget ca-certificates gnupg2 \
  && (wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -) \
  && (echo "deb http://apt.postgresql.org/pub/repos/apt/ buster-pgdg main" | tee  /etc/apt/sources.list.d/pgdg.list) \
  # Update package index with new postgres repo data
  && apt-get update \
  # Finally install postgres
  && apt-get install -y --no-install-recommends postgresql-client-12 \
  # Clean up
  && apt-get clean -y && apt-get autoremove --purge -y && rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/* /var/cache/*

WORKDIR /code
