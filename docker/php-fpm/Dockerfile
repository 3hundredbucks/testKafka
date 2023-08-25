FROM php:8.0-fpm

RUN apt-get update && apt-get install -y \
    wget \
    zlib1g-dev \
    libmcrypt-dev \
    libzip-dev \
    openssl \
    libpng-dev \
    libicu-dev \
    libfreetype6-dev \
    libjpeg-dev

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && docker-php-ext-install pdo pdo_mysql

WORKDIR /var/www/logger

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN apt-get install tzdata

RUN apt-get update --fix-missing \
    && apt-get install -y curl git \
    && rm -rf /var/lib/apt/lists/*

# DEPENDENCIES
RUN apt-get update --fix-missing \
    && apt-get install -y  python \
    && rm -rf /var/lib/apt/lists/*

RUN git clone --depth 1 https://github.com/edenhill/librdkafka.git \
    && ( \
        cd librdkafka \
        && ./configure \
        && make \
        && make install \
    ) \
    && pecl install rdkafka \
    && echo "extension=rdkafka.so" > /usr/local/etc/php/conf.d/rdkafka.ini

RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www