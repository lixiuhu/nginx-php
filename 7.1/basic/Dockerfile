FROM php:7.1-fpm-alpine3.7

###### Install packages
RUN apk --no-cache add g++ gcc git make autoconf curl curl-dev bzip2 bzip2-dev zip icu icu-libs icu-dev \
    libmemcached libmemcached-dev zlib-dev cyrus-sasl-dev mysql-client nginx supervisor tzdata bash \
    libpng-dev freetype-dev libjpeg-turbo-dev libmcrypt-dev \
    postgresql-libs postgresql-dev
RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/

##### Install memcached and redis packages
RUN echo no | pecl install redis && \
    echo yes | pecl install memcached && \
    docker-php-ext-enable redis memcached

##### Install php extension packages
RUN docker-php-ext-install mysqli opcache gd bz2 zip intl pdo_mysql bcmath pgsql pdo_pgsql calendar

###### set up timezone
RUN rm /etc/localtime && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

##install composer
RUN php -r "readfile('https://getcomposer.org/installer');" > composer-setup.php && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer

###### supervisord ######
ADD https://raw.githubusercontent.com/sparkpos/docker-nginx-php/master/conf/supervisord.conf /etc/supervisord.conf
RUN mkdir -p /etc/supervisor/conf.d

###### startup prepare ######
VOLUME ["/var/www/html", "/etc/nignx/site-enabled", "/usr/local/etc"]

ADD https://raw.githubusercontent.com/sparkpos/docker-nginx-php/master/conf/docker-entrypoint.sh /usr/local/bin/docker-entrypoint
RUN chmod +x /usr/local/bin/docker-entrypoint

ENTRYPOINT ["docker-entrypoint"]

EXPOSE 80
WORKDIR /var/www/html

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
