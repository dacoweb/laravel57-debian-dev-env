FROM php:7.2-fpm

ARG HOST_UID=1000
ARG WEB_USER=www-data
ARG WEB_GROUP=www-data
ARG PHP_ROOT_DIR=/usr/local/etc/
ARG XDEBUG_REMOTE_PORT=9100

ENV XDEBUG="false"

# Install general dependencies
RUN apt-get update && \
    apt-get install -y --force-yes --no-install-recommends \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
        openssh-server \
        libmagickwand-dev \
        git \
        cron \
        libxml2-dev \
        unzip \
        iputils-ping

# Install the PHP extentions
RUN docker-php-ext-install pcntl \ 
        soap \
        zip \
        pdo_mysql \
        pdo_pgsql \
        bcmath

# Install the PHP mcrypt extention (from PECL, mcrypt has been removed from PHP 7.2)
RUN pecl install mcrypt-1.0.1
RUN docker-php-ext-enable mcrypt

# Install Imagick:
RUN pecl install imagick && \
    docker-php-ext-enable imagick

# Install the PHP gd library
RUN docker-php-ext-install gd && \
    docker-php-ext-configure gd \
        --enable-gd-native-ttf \
        --with-jpeg-dir=/usr/lib \
        --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd

# Install the xdebug extension
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Install the php memcached extension
RUN pecl install memcached && docker-php-ext-enable memcached

# Install composer
RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer

# source the bash
RUN . ~/.bashrc

# Copy PHP/Files Configurations into containers:
COPY ./config/php.ini ${PHP_ROOT_DIR}/php/
COPY ./config/zz-custom-xdebug.ini ${PHP_ROOT_DIR}/php/conf.d/zz-custom-xdebug.ini
COPY ./config/zz-www.conf ${PHP_ROOT_DIR}/php-fpm.d/zz-www.conf

# Final permissions adjustments:
RUN usermod -u ${HOST_UID} ${WEB_GROUP}

RUN chown -R ${WEB_USER}:${WEB_GROUP} /var/www

# Expose xdebug port ${XDEBUG_REMOTE_PORT}
EXPOSE ${XDEBUG_REMOTE_PORT}
