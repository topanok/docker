FROM php:7.1-fpm

RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -y \
        git \
        unzip \
        libxml2-dev \
        redis-server \
        libzmq3-dev \
        libicu-dev \
        zlib1g-dev \
        libssl-dev \
        libzip-dev \
        pkg-config \
        libpq-dev \
        libpng-dev \
        gnupg2 \
    && yes | apt-get install git-core \
    && yes | apt-get install curl \
    && yes | curl -s https://getcomposer.org/installer | php \
    && yes | mv composer.phar /usr/local/bin/composer \
    && rm -rf /var/lib/apt/lists/*

RUN set -xe \
    && yes | docker-php-ext-configure \
        intl \
    && yes | docker-php-ext-install \
        intl \
        opcache \
        zip \
        soap \
        pdo \
        gd \
        pdo_mysql \
        pdo_pgsql \
    && yes | pecl install \
        xdebug \
    && yes | docker-php-ext-enable \
        xdebug \
        soap

COPY php.ini /usr/local/etc/php/php.ini
RUN sed -i -e "s#TIMEZONE#$timezone#g" /usr/local/etc/php/php.ini

COPY --from=composer:1.6 /usr/bin/composer /usr/bin/composer

# https://getcomposer.org/doc/03-cli.md#composer-allow-superuser
ENV COMPOSER_ALLOW_SUPERUSER 1
# create composer cache directory

RUN mkdir -p /var/www/.composer && chown -R www-data /var/www/.composer

#RUN git clone https://topanok@bitbucket.org/perpspective_studio/foreign-degrau-repository.git ./web

#RUN cd /var/www/web/src && composer install
RUN usermod -u 1000 www-data

WORKDIR /var/www/html