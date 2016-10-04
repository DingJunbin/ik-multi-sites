FROM php:7.0-fpm

MAINTAINER Macken <rystlee100@gmail.com>

#--------------------------------------------------------------------------
# Mandatory Software's Installation
#--------------------------------------------------------------------------

# Installing tools and PHP extentions using "apt", "docker-php", "pecl",
# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng12-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng12-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
        libicu-dev

# Install the PHP mcrypt extention
RUN docker-php-ext-install mcrypt

# Install the PHP mysqli extention
RUN docker-php-ext-install mysqli

# Install the PHP pdo_mysql extention
RUN docker-php-ext-install pdo_mysql

# Install the PHP intl extention
RUN docker-php-ext-install intl && \
    docker-php-ext-enable intl

# Install the PHP gd library
RUN docker-php-ext-configure gd \
        --enable-gd-native-ttf \
        --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd

# Install zip
RUN pecl install zip && \
    docker-php-ext-enable zip 

# Install opcache
RUN docker-php-ext-install opcache && \
    docker-php-ext-enable opcache 

# Install php redis
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini


#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------

ADD ./site.ini /usr/local/etc/php/conf.d
ADD ./site.pool.conf /usr/local/etc/php-fpm.d/

RUN rm -r /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000