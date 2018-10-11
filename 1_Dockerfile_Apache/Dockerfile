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

ADD web /var/www/web
WORKDIR /var/www/web
ADD web-config.conf /etc/apache2/sites-available/

RUN a2ensite web-config.conf
RUN service apache2 start

# Expose apache
EXPOSE 80

CMD /usr/sbin/apache2ctl -D FOREGROUND

# docker run -i -t -p 8001:80 test /bin/bash
