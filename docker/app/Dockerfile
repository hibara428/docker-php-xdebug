FROM composer:2.6.5 as composer

FROM php:8.2.9-fpm-alpine3.18
LABEL maintainer="hibara428 <hibara428@gmail.com>"

# Environments
ENV DEBIAN_FRONTEND=noniteractive
ENV TZ=Asia/Tokyo
ENV COMPOSER_ALLOW_SUPERUSER=1

RUN set -x && \
  apk add -U --no-cache libzip-dev build-base autoconf linux-headers && \
  pecl install xdebug-3.2.0 && \
  docker-php-ext-enable xdebug && \
  printf "xdebug.mode=debug\n\
    xdebug.start_with_request=yes\n\
    xdebug.client_host=host.docker.internal\n\
    xdebug.client_port=9003\n\
    xdebug.log=/tmp/xdebug.log\n\
    xdebug.log_level=0" >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" && \
  mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" && \
  mkdir /app
COPY --from=composer /usr/bin/composer /usr/bin/composer
WORKDIR /app
