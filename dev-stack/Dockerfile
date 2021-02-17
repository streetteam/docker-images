FROM circleci-fullstack-cypress

USER root

RUN nave usemain 14.15.4
RUN apt-get -y install tmux
RUN set -x && \	
    curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose && \	
    chmod +x /usr/bin/docker-compose

EXPOSE 1080
EXPOSE 15672
EXPOSE 2020
EXPOSE 3000
EXPOSE 3200
EXPOSE 3300
EXPOSE 3400
EXPOSE 5000
EXPOSE 5200
EXPOSE 5500
EXPOSE 5600
EXPOSE 5900
EXPOSE 6000
EXPOSE 6200
EXPOSE 9100
EXPOSE 9200

RUN mkdir /code
