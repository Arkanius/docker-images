FROM php:8.2-fpm-alpine3.17

COPY php.ini $PHP_INI_DIR/php.ini

RUN apk --update add sudo && \ 
    adduser -D -u 1000 app && \
    echo "app ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && \
    chmod 0440 /etc/sudoers.d/user && \
    echo "https://dl-cdn.alpinelinux.org/alpine/v3.14/community" >> /etc/apk/repositories && \
    echo "https://dl-cdn.alpinelinux.org/alpine/v3.14/main" >> /etc/apk/repositories && \
    apk add --no-cache wkhtmltopdf ttf-freefont fontconfig \
    linux-headers \
    musl-dev \
    libxml2-dev \
    zlib-dev \
    gmp-dev \
    imap-dev \
    icu-dev \
    postgresql-dev \
    bzip2-dev \
    autoconf \
    build-base \
    curl \
    git \
    subversion \
    freetype-dev \
    libjpeg-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libzip-dev \
    libbz2 \
    libstdc++ \
    libxslt-dev \
    openldap-dev \
    make \
    unzip \
    openssh \
    oniguruma-dev \
    less \
    wget && \
    openssl && \
    docker-php-ext-install zip bz2 mbstring pcntl xsl mysqli pdo pdo_mysql pdo_pgsql bcmath intl sockets exif soap && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install gd && \
    docker-php-ext-configure ldap --with-libdir=lib/ && \
    docker-php-ext-install ldap && \
    pecl install -f xdebug && \
    pecl install -f swoole && \
    docker-php-ext-enable xdebug && \
    docker-php-ext-enable swoole && \
    apk del build-base && \
    rm -rf /var/cache/apk/*

ENV COMPOSER_HOME /composer
ENV PATH /composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1

RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
  && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
  && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
  && php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --2 \
  && rm -rf /tmp/composer-setup.php

USER app

WORKDIR /var/www/app
