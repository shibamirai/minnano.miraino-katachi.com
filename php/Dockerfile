FROM php:7.4.28-fpm

RUN apt-get update \
    && apt-get install -y \
      g++ libbz2-dev libfreetype6-dev libicu-dev libmcrypt-dev libxml2-dev \
      libzip-dev zlib1g-dev locales wget libonig-dev \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure gd --with-freetype \
    && docker-php-ext-configure zip \
    && docker-php-ext-install -j$(nproc) gd zip \
    && localedef -f UTF-8 -i ja_JP ja_JP.UTF-8

COPY --from=composer /usr/bin/composer /usr/bin/composer

ADD ./php.ini /usr/local/etc/php/php.ini
