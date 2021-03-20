FROM php:7.2-fpm-alpine3.12

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk update && apk add \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        zip unzip \
    && docker-php-ext-install \
        mysqli \
        pdo \
        pdo_mysql \
        zip \
        opcache

RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && pecl install -o -f redis \
    && rm -rf /usr/share/php \
    && rm -rf /tmp/* \
    && apk del .phpize-deps

RUN docker-php-ext-enable pdo_mysql redis