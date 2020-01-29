FROM php:7.3-fpm-alpine

COPY php.ini $PHP_INI_DIR/php.ini
    # rm -rf /var/www/app/vendor/h4cc/wkhtmltopdf-amd64/bin/wkhtmltopdf-amd64 && \
    # ln -s /usr/bin/wkhtmltopdf /var/www/app/vendor/h4cc/wkhtmltopdf-amd64/bin/wkhtmltopdf-amd64

RUN apk --update add sudo && \ 
    adduser -D -u 1000 app && \
    echo "app ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/user && \
    chmod 0440 /etc/sudoers.d/user && \
    apk add \
    wkhtmltopdf ttf-freefont fontconfig \
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
    wget && \
    docker-php-ext-install zip bz2 mbstring pcntl xsl mysqli pdo pdo_mysql pdo_pgsql bcmath sockets && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install gd && \
    docker-php-ext-configure ldap --with-libdir=lib/ && \
    docker-php-ext-install ldap && \
    pecl install -f xdebug && \
    docker-php-ext-enable xdebug && \
    apk del build-base && \
    rm -rf /var/cache/apk/*

ENV COMPOSER_HOME /composer
ENV PATH /composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1

RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
  && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
  && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
  && php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer \
  && rm -rf /tmp/composer-setup.php

USER app

WORKDIR /var/www/app
