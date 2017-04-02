# vim: set foldmarker=[[,]] foldlevel=0 foldmethod=marker spell:
# ------------------------------------------------------------------------------
# -> Base image [[
# ------------------------------------------------------------------------------

FROM php:fpm

# ]]
# ------------------------------------------------------------------------------
# -> Install base packages [[
# ------------------------------------------------------------------------------

RUN apt-get update && apt-get install -y \
    git \
    mercurial \
    wget \
    curl \
    ssmtp

# ]]
# ------------------------------------------------------------------------------
# -> Install PHP extensions [[
# ------------------------------------------------------------------------------

RUN docker-php-ext-install \
    mysqli \
    opcache \
    mbstring \
    tokenizer \
    bcmath \
    pdo_mysql

ARG PHP_INSTALL_MCRYPT=false
# Install: mcrypt extension [[
RUN if [ ${PHP_INSTALL_MCRYPT} = true ]; then \
    apt-get -y install libmcrypt-dev \
    && docker-php-ext-install mcrypt \
;fi
# ]]

ARG PHP_INSTALL_XSL=false
# Install: xsl extension [[
RUN if [ ${PHP_INSTALL_XSL} = true ]; then \
    apt-get -y install libxslt1-dev \
    && docker-php-ext-install xsl \
;fi
# ]]

ARG PHP_INSTALL_INTL=false
# Install: intl extension [[
RUN if [ ${PHP_INSTALL_INTL} = true ]; then \
    apt-get -y install libicu-dev \
    && docker-php-ext-install intl \
;fi
# ]]

ARG PHP_INSTALL_GD=false
# Install: gd extension [[
RUN if [ ${PHP_INSTALL_GD} = true ]; then \
    apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
    && docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
;fi
# ]]

ARG PHP_INSTALL_SOAP=false
# Install: soap extension [[
RUN if [ ${PHP_INSTALL_SOAP} = true ]; then \
    apt-get -y install libxml2-dev \
    && docker-php-ext-install soap \
;fi
# ]]

ARG PHP_INSTALL_ZIP_ARCHIVE=false
# Install: zip extension [[
RUN if [ ${PHP_INSTALL_ZIP_ARCHIVE} = true  ]; then \
    apt-get -y install zlib1g-dev \
    && docker-php-ext-install zip \
;fi
# ]]

ARG PHP_INSTALL_MEMCACHED=false
# Install: memcached extension [[
RUN if [ ${PHP_INSTALL_MEMCACHED} = true ]; then \
    apt-get -y install libmemcached-dev \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
;fi
# ]]

ARG PHP_INSTALL_MONGO=false
# Install: mongodb extension [[
RUN if [ ${PHP_INSTALL_MONGO} = true ]; then \
    apt-get install -y libssl-dev \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb \
;fi
# ]]

# ]]
# ------------------------------------------------------------------------------
# -> Install Xdebug [[
# ------------------------------------------------------------------------------

ARG INSTALL_XDEBUG=false
ARG XDEBUG_VERSION=2.5.1

RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    curl -LOk http://xdebug.org/files/xdebug-$XDEBUG_VERSION.tgz \
    && tar -zxvf xdebug-$XDEBUG_VERSION.tgz && cd xdebug-$XDEBUG_VERSION \
    && phpize && ./configure --enable-xdebug && make && make install \
;fi

# ]]
# ------------------------------------------------------------------------------
# -> Composer [[
# ------------------------------------------------------------------------------

ARG INSTALL_COMPOSER=false

RUN if [ ${INSTALL_COMPOSER} = true ]; then \
    curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
    && php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer \
    && rm -rf /tmp/composer-setup.php \
;fi

# ]]
# ------------------------------------------------------------------------------
# -> PHPunit [[
# ------------------------------------------------------------------------------

ARG INSTALL_PHPUNIT=false
ARG PHPUNIT_VERSION=6.0

RUN if [ ${INSTALL_PHPUNIT} = true ]; then \
    curl -L -o phpunit https://phar.phpunit.de/phpunit-$PHPUNIT_VERSION.phar \
    && chmod +x ./phpunit \
    && mv ./phpunit /usr/local/bin/ \
;fi

# ]]
# ------------------------------------------------------------------------------
# -> Magerun [[
# ------------------------------------------------------------------------------

ARG INSTALL_MAGERUN=false

RUN if [ ${INSTALL_MAGERUN} = true ]; then \
    curl -o magerun https://raw.githubusercontent.com/netz98/n98-magerun/master/n98-magerun.phar \
    && chmod +x ./magerun \
    && mv ./magerun /usr/local/bin/ \
;fi

# ]]
# ------------------------------------------------------------------------------
# -> Magerun 2 [[
# ------------------------------------------------------------------------------

ARG INSTALL_MAGERUN_2=false

RUN if [ ${INSTALL_MAGERUN_2} = true ]; then \
    curl -o magerun2 https://files.magerun.net/n98-magerun2.phar \
    && chmod +x ./magerun2 \
    && mv ./magerun2 /usr/local/bin/ \
;fi

# ]]
# ------------------------------------------------------------------------------
# -> SQLite [[
# ------------------------------------------------------------------------------

ARG INSTALL_SQLITE=false

RUN if [ ${INSTALL_SQLITE} = true ]; then \
    apt-get update && apt-get install -y \
        sqlite \
;fi

# ]]
# ------------------------------------------------------------------------------
# -> Configure [[
# ------------------------------------------------------------------------------

# Set up config files
COPY config/php.ini /usr/local/etc/php/php.ini
COPY config/ssmtp.conf /etc/ssmtp/ssmtp.conf

# Change working directory
WORKDIR /var/www/project

# ]]
# ------------------------------------------------------------------------------
# -> Users [[
# ------------------------------------------------------------------------------

# Add & change user uid
RUN useradd --shell /bin/bash -u 1024 -o -c "" -m devbox
RUN usermod -u 1000 devbox

USER devbox

# ]]
# ------------------------------------------------------------------------------