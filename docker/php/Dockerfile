FROM php:8.0-fpm

# Composer install
COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

# install Node.js
COPY --from=node:16.13 /usr/local/bin /usr/local/bin
COPY --from=node:16.13 /usr/local/lib /usr/local/lib

# setup environment
ENV TZ=Asia/Tokyo \
  LANG=en_US.UTF-8 \
  LANGUAGE=en_US:en \
  LC_ALL=en_US.UTF-8 \
  COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

RUN apt-get -y update; apt-get install ca-certificates
RUN apt-get -y install wget git zip unzip libpq-dev libicu-dev libonig-dev libzip-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev locales vim
RUN apt-get clean
RUN locale-gen en_US.UTF-8
RUN localedef -f UTF-8 -i en_US en_US.UTF-8
RUN git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis
RUN docker-php-ext-install pdo_mysql zip bcmath redis
RUN composer config -g process-timeout 3600
RUN composer config -g repos.packagist composer https://packagist.org

RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd

COPY ./docker/php/php.ini /usr/local/etc/php/php.ini

WORKDIR /var/www/html