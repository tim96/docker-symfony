# See https://github.com/docker-library/php/blob/master/7.2/fpm/Dockerfile
# See https://github.com/docker-library/php/blob/master/7.3/fpm/Dockerfile
FROM php:7.3-fpm
ARG TIMEZONE

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    unzip \
    make \
    curl \
    htop \
    mc \
    nano \
    bash \
    gcc \
    make \
    re2c \
    sudo \
    vim \
    autoconf \
    build-essential \
    libbsd-dev \
    libbz2-dev \
    libc-client2007e-dev \
    libc6-dev \
    # libcurl3 \
    libcurl4 \
    libcurl4-openssl-dev \
    libedit-dev \
    libedit2 \
    libgmp-dev \
    libgpgme11-dev \
    libicu-dev \
    libjpeg-dev \
    libkrb5-dev \
    libldap2-dev \
    libmagick++-dev \
    libmagickwand-dev \
    libmemcached-dev \
    libpcre3-dev \
    libsqlite3-0 \
    libsqlite3-dev \
    libssh2-1-dev \
    libssl-dev \
    libtinfo-dev \
    libtool \
    libvpx-dev \
    libwebp-dev \
    libxml2 \
    libxml2-dev \
    libxpm-dev \
    libxslt1-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libpq-dev \
    librabbitmq-dev \
    libssl-dev \
    libzip-dev \
    zlib1g-dev
    # libressl-dev \
    # libssl1.0-dev \
    # libssh-dev \

RUN docker-php-ext-install -j$(nproc) iconv \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
	&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install imap \
    && docker-php-ext-install \
        bcmath \
        bz2 \
        calendar \
        dba \
        exif \
        gettext \
        gmp \
        intl \
        ldap \
        mbstring \
        mysqli \
        opcache \
        pcntl \
        pdo \
        pdo_mysql \
        pdo_pgsql \
        pdo_sqlite \
        pgsql \
        shmop \
        soap \
        sockets \
        sysvmsg \
        sysvsem \
        sysvshm \
        wddx \
        xmlrpc \
        xsl \
        zip \
        tokenizer

# RUN php -v
# RUN phpize -v

RUN pecl install apcu && docker-php-ext-enable apcu
# RUN echo "extension=apcu.so" > /usr/local/etc/php/conf.d/apcu.ini

# For phalcon 4.0.4
RUN pecl install psr && docker-php-ext-enable psr

RUN pecl install memcached && docker-php-ext-enable memcached
RUN pecl install redis && docker-php-ext-enable redis
RUN pecl install imagick && docker-php-ext-enable imagick
RUN pecl install gnupg && docker-php-ext-enable gnupg
RUN pecl install swoole && docker-php-ext-enable swoole
RUN pecl install mongodb && docker-php-ext-enable mongodb
RUN pecl install amqp && docker-php-ext-enable amqp
RUN pecl install uuid && docker-php-ext-enable uuid

ARG INSTALL_PHALCON=false
ARG PHALCON_VERSION
ENV PHALCON_VERSION ${PHALCON_VERSION}

# todo: move 7.2 to new constant
RUN if [ $INSTALL_PHALCON = true ]; then \
    curl -L -o /tmp/cphalcon.zip https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.zip \
    && unzip -d /tmp/ /tmp/cphalcon.zip \
    && cd /tmp/cphalcon-${PHALCON_VERSION}/build \
    && ./install \
    && docker-php-ext-enable phalcon \
    # && echo "extension=phalcon.so" >> /etc/php/7.2/mods-available/phalcon.ini \
    # && ln -s /etc/php/7.2/mods-available/phalcon.ini /etc/php/7.2/cli/conf.d/30-phalcon.ini \
    && rm -rf /tmp/cphalcon* \
;fi

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

ARG INSTALL_ZEPHIR=false
RUN if [ $INSTALL_ZEPHIR = true ]; then \
    # todo: add env param for zephir version
    curl -L -o /usr/local/bin/zephir https://github.com/phalcon/zephir/releases/download/0.12.17/zephir.phar && \
    chmod +x /usr/local/bin/zephir && \
    zephir --version && \
    zephir help \
;fi

COPY ./php.ini /usr/local/etc/php/conf.d

# install xdebug
#RUN pecl install xdebug && docker-php-ext-enable xdebug
# Disable xdebug
#RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# RUN echo 'alias sf="php app/console"' >> ~/.bashrc
RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

# TODO: provide better description in Readme.md!!
# For better performance we can move var/cache folder from sync between host and container
RUN mkdir -p /srv/var/cache/dev
RUN mkdir -p /srv/var/cache/prod
RUN mkdir -p /srv/var/log
# TODO: need to replace to nobody user permissions
RUN chmod 777 -R /srv/var

WORKDIR /srv/app
