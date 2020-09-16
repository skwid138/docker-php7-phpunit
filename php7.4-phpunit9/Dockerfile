FROM php:7.4-cli
MAINTAINER Hunter Rancourt <hunter.rancourt@gmail.com>
ENV DEBIAN_FRONTEND noninteractive

# Install required packages
RUN apt-get update -yqq \
    && apt-get install git zlib1g-dev libsqlite3-dev libzip-dev -y \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_sqlite

# Install Composer, require PHPUnit 9.*, and require Mockery 1.4
RUN curl -fsSL https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer global require phpunit/phpunit ^9.0 --no-progress --no-scripts --no-interaction \
    && composer global require --dev mockery/mockery ^1.4 --no-progress --no-scripts --no-interaction


# Install Xdebug
RUN pecl install xdebug \
    && echo 'zend_extension="/usr/local/lib/php/extensions/no-debug-non-zts-20190902/xdebug.so"' > \
        /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && php -m | grep xdebug

# Update $PATH
ENV PATH /root/.composer/vendor/bin:$PATH

# Run PHPUnit
CMD ["phpunit"]
