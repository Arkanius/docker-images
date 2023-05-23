FROM php:8.2-fpm-alpine3.17

RUN apk update && \ 
        apk add alpine-sdk \
        autoconf \
        bzip2-dev \
        zlib-dev \
        jpeg-dev \
        libpng-dev \
        gettext-dev \
        libxml2-dev \
        libzip-dev \
        sqlite-dev \
        linux-headers \
        libxslt-dev && \
        openssl && \
        echo "https://dl-cdn.alpinelinux.org/alpine/v3.14/community" >> /etc/apk/repositories && \
        echo "https://dl-cdn.alpinelinux.org/alpine/v3.14/main" >> /etc/apk/repositories 

RUN apk add --no-cache wkhtmltopdf ttf-freefont fontconfig && \ 
    docker-php-ext-install bz2 calendar exif gettext mysqli pcntl shmop sockets sysvmsg sysvsem sysvshm zip pdo_mysql pdo_sqlite xsl bcmath soap && \
    docker-php-ext-configure gd --with-jpeg && docker-php-ext-install gd && \
    printf "\n" | pecl install apcu && printf "\n" | pecl install redis && \
    printf "\n" | pecl install swoole && \
    docker-php-ext-enable apcu && docker-php-ext-enable redis && docker-php-ext-enable swoole && apk del alpine-sdk less

## to do image resize fix the line below
# RUN docker-php-ext-configure gd --with-webp && apk del alpine-sdk less 

RUN apk update && apk add ca-certificates ffmpeg libwebp libwebp-tools supervisor nginx sqlite git openssh curl wkhtmltopdf ttf-freefont ttf-dejavu fontconfig && \
    curl -sS https://getcomposer.org/installer -o composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer --2 && rm -rf composer-setup.php && \
    cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini && \
    sed -i 's/expose_php = On/expose_php = Off/g' /usr/local/etc/php/php.ini && \
    sed -i 's/memory_limit = 128M/memory_limit = 4G/g' /usr/local/etc/php/php.ini && \
    sed -i 's/max_execution_time = 30/max_execution_time = 600/g' /usr/local/etc/php/php.ini
    
RUN mkdir -p /var/log/supervisor /run/nginx

COPY supervisord.conf /etc/supervisord.conf
COPY default.conf /etc/nginx/http.d/default.conf
COPY php.ini /usr/local/etc/php/php.ini

RUN apk add nodejs nghttp2-dev npm && npm install -g pm2

EXPOSE 80

WORKDIR /var/www/app

CMD ["/usr/bin/supervisord"]
