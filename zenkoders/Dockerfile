FROM node:6
MAINTAINER Jibran Kalia <jibran.kalia@gmail.com>

WORKDIR /opt/zenkoders

RUN apt-get update \
    && apt-get install -y jq python git build-essential supervisor --no-install-recommends \
    && mkdir -p /var/log/supervisor

WORKDIR /opt/zenkoders
RUN git clone https://github.com/JibranKalia/zenkoders.git
WORKDIR /opt/zenkoders/zenkoders
RUN npm install

EXPOSE 8080

CMD npm start
