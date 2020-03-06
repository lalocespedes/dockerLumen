FROM php:7.2-fpm

RUN apt-get update

RUN apt-get install -y \
  curl \
  git \
  supervisor \
  zip \
  unzip

# lumen packages

RUN apt-get install -y libpng-dev libxml2-dev libxslt-dev \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install tokenizer \
    && docker-php-ext-install pdo \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install gd \
    && docker-php-ext-install xsl \
    && docker-php-ext-install soap

RUN pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY php.ini /usr/local/etc/php/conf.d/40-custom.ini

# Install lumen php

WORKDIR /var/www/html
COPY . /var/www/html

CMD ["php", "-S", "0.0.0.0:9090", "-t", "/var/www/html/public"]

EXPOSE 9090