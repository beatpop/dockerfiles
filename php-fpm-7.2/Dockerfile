FROM php:7.2.10-fpm
MAINTAINER beatpop "huangzq@suancloud.cn"

ENV PHPREDIS_VERSION php7.2.10

ADD sources.list /etc/apt/

# Install modules
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libicu-dev \
        libxslt-dev \
        g++ \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && apt-get install -y libicu-dev \
    && docker-php-ext-install intl \
    && :\
    && apt-get install -y libxml2-dev \
    && apt-get install -y libxslt-dev \
    && docker-php-ext-install soap \
    && docker-php-ext-install xsl \
    && docker-php-ext-install xmlrpc \
    && docker-php-ext-install wddx \
    && apt-get install -y libbz2-dev \
    && docker-php-ext-install bz2 \
    && docker-php-ext-install zip \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install exif \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install calendar \
    && docker-php-ext-install sockets \
    && docker-php-ext-install gettext \
    && docker-php-ext-install shmop \
    && docker-php-ext-install sysvmsg \
    && docker-php-ext-install sysvsem \
    && docker-php-ext-install sysvshm \
    && docker-php-ext-install opcache \
    # redis-php
    && pecl install -o -f redis \
    && docker-php-ext-enable redis \
    # xdebug
    && pecl install -o -f xdebug \
    && docker-php-ext-enable xdebug 

# php.ini
COPY php.ini /usr/local/etc/php/

# opcache
# COPY conf.d/opcache.ini /usr/local/etc/php/conf.d/

# composer
ENV REFRESHED_COMPOSER_AT 2018091623_0942
COPY composer.phar /usr/local/bin/composer
RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com

CMD ["php-fpm"]