# Alpine 3.11 using PHP 7.4 (Official PHP Image)

FROM php:7.4-fpm-alpine

RUN apk update && apk add alpine-sdk autoconf bzip2-dev zlib-dev jpeg-dev libpng-dev gettext-dev libxml2-dev libzip-dev sqlite-dev libxslt-dev && \
    docker-php-ext-install bz2 calendar exif gettext mysqli pcntl shmop sockets sysvmsg sysvsem sysvshm zip pdo_mysql pdo_sqlite xsl bcmath && \
    docker-php-ext-configure gd --with-jpeg && docker-php-ext-install gd && \
    printf "\n" | pecl install apcu && printf "\n" | pecl install redis && \
    printf "\n" | pecl install xdebug && printf "\n" | pecl install swoole && \
    docker-php-ext-enable apcu && docker-php-ext-enable redis && docker-php-ext-enable swoole && \
    apk del alpine-sdk

RUN apk update && apk add supervisor nginx sqlite git openssh curl wkhtmltopdf ttf-freefont ttf-ubuntu-font-family ttf-dejavu fontconfig && \
    curl -sS https://getcomposer.org/installer -o composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer --2 && rm -rf composer-setup.php && \
    cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini && \
    sed -i 's/expose_php = On/expose_php = Off/g' /usr/local/etc/php/php.ini && \
    sed -i 's/memory_limit = 128M/memory_limit = 4G/g' /usr/local/etc/php/php.ini && \
    sed -i 's/max_execution_time = 30/max_execution_time = 600/g' /usr/local/etc/php/php.ini && \
    ln -s /usr/local/sbin/php-fpm /usr/sbin/php-fpm7 && mkdir -p /var/log/supervisor /run/nginx

COPY supervisord.conf /etc/supervisord.conf
COPY default.conf /etc/nginx/conf.d/default.conf
COPY php.ini /usr/local/etc/php/php.ini

RUN apk add nodejs nghttp2-dev npm && npm install -g pm2

EXPOSE 80

WORKDIR /var/www/app

CMD ["/usr/bin/supervisord"]
