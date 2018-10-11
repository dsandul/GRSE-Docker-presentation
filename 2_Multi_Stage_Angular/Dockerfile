##########################
#stage 1 - build
##########################

FROM node:8.11.2-alpine as node

WORKDIR /usr/src/app

COPY ./angular-docker/package*.json ./

RUN npm install

COPY ./angular-docker/ .

RUN npm run build


##########################
#stage 2 - run application
##########################

FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y tzdata

RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y \
      apache2 \
	  nano

# Manually set up the apache environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

COPY --from=node /usr/src/app/dist /var/www
WORKDIR /var/www
ADD web-config.conf /etc/apache2/sites-available/

RUN a2ensite web-config.conf
RUN service apache2 start

# Expose apache
EXPOSE 80

CMD /usr/sbin/apache2ctl -D FOREGROUND

# docker run -i -t -p 8001:80 test /bin/bash
