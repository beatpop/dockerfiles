FROM php:7.4.3-cli
MAINTAINER beatpop "niucha@qingmutec.com"

ENV PHPREDIS_VERSION php:7.4.3-cli

ADD sources.list /etc/apt/

# Install modules
RUN buildDeps01="libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev libicu-dev libxslt-dev libxml2-dev " \ 
    && buildDeps02="gcc g++ libzip-dev unzip wget libpcre3 libpcre3-dev ruby zlib1g zlib1g.dev libmemcached-dev " \
    && apt-get update && apt-get install -y $buildDeps01 $buildDeps02 \
#    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install intl \
    && docker-php-ext-install soap \
    && docker-php-ext-install xsl \
    && docker-php-ext-install zip \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install gettext \
    && docker-php-ext-install opcache \
    && docker-php-ext-install sockets \
    #安装igbinary扩展
    && pecl install -o -f igbinary \ 
    && rm -rf /tmp/pear \ 
    && docker-php-ext-enable igbinary \
    # redis-php
    && pecl install -o -f redis-5.0.2 \
    && rm -rf /tmp/pear \ 
    && docker-php-ext-enable redis \
    # rdkafka
    # && pecl install -o -f rdkafka \
    # && rm -rf /tmp/pear \ 
    # && docker-php-ext-enable rdkafka \
    # swoole
    && pecl install -o -f swoole-4.4.8 \
    && rm -rf /tmp/pear \ 
    && docker-php-ext-enable swoole \
    # xlswriter
    && pecl install -o -f xlswriter \
    && rm -rf /tmp/pear \ 
    && docker-php-ext-enable xlswriter \
    # memcached 
    && pecl install -o -f memcached \
    && rm -rf /tmp/pear \ 
    && docker-php-ext-enable memcached 

# php.ini
COPY php.ini /usr/local/etc/php/

# composer
COPY composer.phar /usr/local/bin/composer
RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com

EXPOSE 9000

#CMD ["php", "-m"]
WORKDIR /var/www/html