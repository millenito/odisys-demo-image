FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Jakarta

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y software-properties-common \
    && add-apt-repository ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y \
    vim \
    apache2 \
    php7.3 \
    php7.3-pgsql \
    php7.3-gd \
    && apt-get clean

RUN a2enmod rewrite \
    && a2enmod headers \
    && sed -i 's/AllowOverride None/AllowOverride all/' /etc/apache2/apache2.conf \
    && service apache2 restart

EXPOSE 80
CMD apachectl -D FOREGROUND
