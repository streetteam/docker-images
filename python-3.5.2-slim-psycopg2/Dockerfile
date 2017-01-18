FROM python:3.5.2-slim

# We install apt deps + postgres dependencies, so the response can be cached,
# and remove the deps afterwards, to keep image size down.
RUN set -x \
    && apt-get update -qq \
    && apt-get install -y build-essential libpq-dev \
    && pip install 'psycopg2==2.6.2' \
    && apt-get autoremove -y build-essential \
    && apt-get clean
