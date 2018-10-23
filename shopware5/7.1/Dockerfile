FROM php:7.1-fpm-alpine3.7
MAINTAINER Linus Lotz<l.lotz@reply.de>
ENV RUN_DEPS="bash icu-libs libgd libintl libxml2 mariadb-client-libs postgresql-libs"
ENV BUILD_DEPS="${PHPIZE_DEPS} freetype-dev icu-dev libjpeg-turbo-dev libpng-dev libwebp-dev libxml2-dev libxpm-dev mariadb-dev postgresql-dev zlib-dev"
ENV PECL_EXTS="redis"
ENV PHP_EXTS="gd intl mysqli opcache pdo_mysql pdo_pgsql soap zip"
RUN    apk update \
    && apk upgrade \
    && apk add --no-cache $RUN_DEPS \
    && apk add -t .build-deps $BUILD_DEPS \
    && docker-php-ext-install $PHP_EXTS \
    && pecl install -o -f $PECL_EXTS \
    && docker-php-ext-enable $PECL_EXTS \
    && apk del .build-deps \
    # Clear
    && rm -rf /tmp/* /var/cache/apk/*

RUN    mkdir -p setup \
    && cd setup  \
    && curl -sSL https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz -o ioncube.tar.gz \
    && tar -xf ioncube.tar.gz \
    && mv ioncube/ioncube_loader_lin_7.1.so /usr/local/lib/php/extensions/no-debug-non-zts-20160303/ \
    && echo 'zend_extension = /usr/local/lib/php/extensions/no-debug-non-zts-20160303/ioncube_loader_lin_7.1.so' >  /usr/local/etc/php-fpm.d/20-ioncube.conf \
    && rm -rf setup

COPY --from=composer:1.7 /usr/bin/composer /usr/bin/composer
