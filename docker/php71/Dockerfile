FROM php:7.1-fpm-alpine

# mcrypt, gd, iconv
RUN apk add --update --no-cache \
        freetype-dev \
        libjpeg-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" gd

# gmp
RUN apk add --update --no-cache gmp gmp-dev \
    && docker-php-ext-install gmp

# Bz2
RUN apk add --update --no-cache libbz2 bzip2-dev \
    && docker-php-ext-install bz2

# intl, zip, soap, xml, bz2, bcmath
RUN apk add --update --no-cache libintl icu icu-dev libxml2-dev \
    && docker-php-ext-install intl zip soap xml bcmath

# php-redis
ENV PHPREDIS_VERSION 3.1.2

RUN docker-php-source extract \
    && curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
    && docker-php-ext-install redis \
    && docker-php-source delete

# imagick
RUN apk add --update --no-cache autoconf g++ imagemagick-dev libtool make pcre-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && apk del autoconf g++ libtool make pcre-dev


# mysqli, pdo, pdo_mysql
RUN apk add --update --no-cache postgresql-dev \
    && docker-php-ext-install mysqli pdo pdo_mysql

# pdo_pgsql
RUN apk add --update --no-cache postgresql-dev \
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-install pgsql

# graphviz
RUN apk add --update --no-cache \
        graphviz \
        ttf-freefont

# composer
RUN curl --silent --show-error https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
