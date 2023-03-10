FROM composer:latest as composer
FROM php:8.2-fpm as php

RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
            curl \
            exif \
            git \
            libmemcached-dev \
            libz-dev \
            libpq-dev \
            libjpeg-dev \
            libpng-dev \
            libfreetype6-dev \
            libssl-dev \
            libwebp-dev \
            libxpm-dev \
            libmcrypt-dev \
            libonig-dev \
            zip \
            unzip; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    # Install the PHP pdo_pgsql extention
    docker-php-ext-install pdo_pgsql; \
    # Install the PHP gd library
    docker-php-ext-configure gd \
            --prefix=/usr \
            --with-jpeg \
            --with-webp \
            --with-xpm \
            --with-freetype; \
    docker-php-ext-install gd; \
    php -r 'var_dump(gd_info());'

RUN docker-php-ext-install exif

ENV XDEBUG_CONFIG="client_host=host.docker.internal client_port=9001 start_with_request=yes"
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV COMPOSER_MEMORY_LIMIT=-1

COPY --from=composer /usr/bin/composer /usr/local/bin/composer
RUN /usr/local/bin/composer self-update

RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/sh -g www www
USER www

WORKDIR /var/www/app