FROM tigerseye/nginx-php:7.1-basic

ENV DRUSH_VERSION 8.1.16
ENV DRUSH_LAUCHER 0.6.0

###### install drush ######
RUN curl -fsSL -o /usr/local/bin/drush8 https://github.com/drush-ops/drush/releases/download/$DRUSH_VERSION/drush.phar | sh && \
    chmod +x /usr/local/bin/drush8 && \
    drush8 core-status 

###### install drush laucher ######
RUN wget -O drush.phar https://github.com/drush-ops/drush-launcher/releases/download/$DRUSH_LAUCHER/drush.phar && \
    chmod +x drush.phar && \
    mv drush.phar /usr/local/bin/drush
ENV DRUSH_LAUNCHER_FALLBACK=/usr/local/bin/drush8

###### change php config ######
ADD ./conf/php.ini /usr/local/etc/php/
RUN sed -i -e "s/listen = 9000//g" /usr/local/etc/php-fpm.d/zz-docker.conf
RUN sed -i -e "s/access.log = \/proc\/self\/fd\/2/access.log = \/proc\/self\/fd\/1/g" /usr/local/etc/php-fpm.d/docker.conf
ADD ./conf/www.conf /usr/local/etc/php-fpm.d/www.conf

#Update nginx config
RUN mkdir /etc/nginx/ssl /etc/nginx/site-enabled
ADD ./conf/nginx.conf /etc/nginx/
ADD ./conf/drupal8.conf /etc/nginx/site-enabled/
