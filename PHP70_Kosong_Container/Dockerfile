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
    git \
    wget \
    unzip \
    php7.3 \
    php7.3-pgsql \
    php7.3-mysql \
    php7.3-gd \
    php7.3-dev \
    php7.3-xml \
    php-pear \
    && apt-get clean

RUN a2enmod rewrite \
    && a2enmod headers \
    && sed -i 's/AllowOverride None/AllowOverride all/' /etc/apache2/apache2.conf \
    && service apache2 restart

# download & unzip instantclient12.1
RUN mkdir /opt/oracle \
    && cd /opt/oracle \
    && wget https://github.com/pwnlabs/oracle-instantclient/raw/master/instantclient-basic-linux.x64-18.5.0.0.0dbru.zip \
    && wget https://github.com/pwnlabs/oracle-instantclient/raw/master/instantclient-sdk-linux.x64-18.5.0.0.0dbru.zip \
    && unzip instantclient-basic-linux.x64-18.5.0.0.0dbru.zip \
    && unzip instantclient-sdk-linux.x64-18.5.0.0.0dbru.zip

WORKDIR /opt/oracle

# setup instantclient
# RUN ln -s /opt/oracle/instantclient_18_5/libclntsh.so.18.1 /opt/oracle/instantclient_18_5/libclntsh.so \
#     && ln -s /opt/oracle/instantclient_18_5/libocci.so.18.1 /opt/oracle/instantclient_18_5/libocci.so \
RUN echo /opt/oracle/instantclient_18_5 > /etc/ld.so.conf.d/oracle-instantclient \
    && ldconfig

# install oci8
RUN echo "instantclient,/opt/oracle/instantclient_18_5" | pecl install oci8 \
    && echo "extension = oci8.so" >> /etc/php/7.3/apache2/php.ini \
    && echo "extension = oci8.so" >> /etc/php/7.3/cli/php.ini \
    && service apache2 restart \
    && echo "/opt/oracle/instantclient_18_5" >> /etc/ld.so.conf.d/x86_64-linux-gnu.conf \
    && ldconfig \
    && rm -f /opt/oracle/*.zip

# Install phpredis
#RUN pecl install redis \
    #&& echo "extension=redis.so" >> /etc/php/7.3/apache2/php.ini \
    #&& service apache2 restart

EXPOSE 80
CMD apachectl -D FOREGROUND
