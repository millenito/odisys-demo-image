FROM nginx:latest

RUN apt-get -y update \
    && apt-get -y install \
    vim \
    php \
    php7.0-pgsql \
    php-fpm

WORKDIR /usr/share/nginx/html

RUN rm -f /etc/nginx/conf.d/default.conf \
    && rm -f /usr/share/nginx/html/*

COPY index.php .

COPY docker-webserver/php/www.conf /etc/php/7.0/fpm/pool.d/www.conf
COPY docker-webserver/nginx/nginx.conf /etc/nginx/nginx.conf
COPY docker-webserver/nginx/conf.d/. /etc/nginx/conf.d/.

EXPOSE 80
EXPOSE 443
EXPOSE 9053

CMD service php7.0-fpm start && nginx -g 'daemon off;'
