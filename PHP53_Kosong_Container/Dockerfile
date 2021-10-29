FROM ubuntu:12.04

VOLUME ["/var/www"]

RUN sed -i -e 's/archive.ubuntu.com\|security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
	apache2\
	php5 \
	php5-cli\
	libapache2-mod-php5 \
        php5-gd \
	php5-ldap \
        php5-mysql \
        php5-pgsql \
        php5-dev \
        php5-curl \
        php5-xdebug \
        php-pear \
        build-essential \
        libaio1 \
        vim \
        git \
        wget \
        unzip \
        && apt-get clean
      
# download & unzip instantclient12.1
RUN mkdir /opt/oracle \
    && cd /opt/oracle \
    && wget https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-basic-linux.x64-12.1.0.2.0.zip \
    && wget https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-sdk-linux.x64-12.1.0.2.0.zip \
    && unzip instantclient-basic-linux.x64-12.1.0.2.0.zip \
    && unzip instantclient-sdk-linux.x64-12.1.0.2.0.zip

WORKDIR /opt/oracle

# setup instantclient
RUN ln -s /opt/oracle/instantclient_12_1/libclntsh.so.12.1 /opt/oracle/instantclient_12_1/libclntsh.so \
    && ln -s /opt/oracle/instantclient_12_1/libocci.so.12.1 /opt/oracle/instantclient_12_1/libocci.so \
    && echo /opt/oracle/instantclient_12_1 > /etc/ld.so.conf.d/oracle-instantclient \
    && ldconfig

# install oci8
RUN echo "instantclient,/opt/oracle/instantclient_12_1" | pecl install oci8-2.0.12 \
    && echo "extension = oci8.so" >> /etc/php5/apache2/php.ini \
    && echo "extension = oci8.so" >> /etc/php5/cli/php.ini \
    && service apache2 restart \
    && echo "/opt/oracle/instantclient_12_1" >> /etc/ld.so.conf.d/x86_64-linux-gnu.conf \
    && ldconfig \
    && rm -f /opt/oracle/*.zip

# apache
COPY apache_default /etc/apache2/sites-available/default
RUN a2enmod rewrite

RUN echo "xdebug.remote_enable=on" >> /etc/php5/conf.d/xdebug.ini \
        && echo "xdebug.remote_handler=dbgp" >> /etc/php5/conf.d/xdebug.ini \
        && echo "xdebug.remote_host=localhost" >> /etc/php5/conf.d/xdebug.ini \
        && echo "xdebug.remote_port=9000" >> /etc/php5/conf.d/xdebug.ini \
        service apache2 restart
# php settings
# RUN sed -i "s|;listen.owner = www-data|listen.owner = www-data|g" /etc/php5/fpm/pool.d/www.conf \
#     && sed -i "s|;listen.group = www-data|listen.group = www-data|g" /etc/php5/fpm/pool.d/www.conf \
#     && sed -i "s|;date.timezone =|date.timezone = 'Asia/Jakarta'|g" /etc/php5/fpm/php.ini
#
# RUN mkdir /usr/share/nginx/html \
#     && rm -f /etc/nginx/sites-available/* \
#     && rm -f /etc/nginx/sites-enabled/*
#
# WORKDIR /usr/share/nginx/html
#
# COPY docker-webserver/nginx/nginx.conf /etc/nginx/nginx.conf
# COPY docker-webserver/nginx/sites-available/ /etc/nginx/sites-available/
#
# RUN ln -sv /etc/nginx/sites-available/* /etc/nginx/sites-enabled/.

EXPOSE 80
EXPOSE 9000

CMD apachectl -D FOREGROUND
