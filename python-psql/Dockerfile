FROM python:3.7.10-slim

RUN apt-get update
RUN apt-get install -y

# Install Postgres12
RUN apt-get install -y wget ca-certificates gnupg2
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ buster-pgdg main" | tee  /etc/apt/sources.list.d/pgdg.list
RUN apt-get update
RUN apt-get -y install postgresql-client-12 postgresql-12-postgis-2.5

WORKDIR /code
