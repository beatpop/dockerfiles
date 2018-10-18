FROM php:7-fpm
MAINTAINER Chad Cai "chad@suancloud.cn"

ENV PHPREDIS_VERSION php7
ENV REFRESHED_BUILTIN_EXT_AT 20160111_1653

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
    && docker-php-ext-install gd \ 
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install exif \
    && docker-php-ext-install xsl \
    && docker-php-ext-install bcmath \
    # redis-php
    && docker-php-source extract \
    && curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
    && echo redis >> /usr/src/php-available-exts \
    && docker-php-ext-install redis \
    && docker-php-source delete \
    # xdebug
    && pecl install xdebug

# php.ini
ENV REFRESHED_PHPINI_AT 2016120123_0942
COPY php.ini /usr/local/etc/php/

# opcache
COPY conf.d/opcache.ini /usr/local/etc/php/conf.d/

# composer
ENV REFRESHED_COMPOSER_AT 2016120123_0942
COPY composer.phar /usr/local/bin/composer
RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com
# 更改社区的源（上面的由于需要验证用户密码）
# RUN composer config -g repo.packagist composer https://packagist.laravel-china.org

CMD ["php-fpm"]