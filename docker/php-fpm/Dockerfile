FROM php:fpm

ARG NODE_VERSION=16

WORKDIR /var/www

RUN apt-get update

# Установка дополнительных пакетов для PHP-расширений
RUN apt-get -y install curl unzip libbz2-dev sqlite3 libsqlite3-dev libldb-dev \
        libssl-dev libffi-dev libgmp-dev libc-client-dev libkrb5-dev libldap2-dev \
        libcurl4-openssl-dev libjpeg-dev libonig-dev libreadline-dev libsodium-dev \
        libtidy-dev libxslt-dev libzip-dev libfbclient2 libib-util \
        libpq-dev libonig-dev libxml2-dev libmemcached-dev \
        libzip-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev

# Подготавливаем PHP-расширения
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl

# Собираем PHP-расширения
RUN docker-php-ext-install bcmath bz2 calendar ctype curl dba \
        exif ffi fileinfo filter \
        ftp gd gettext gmp iconv imap \
        intl ldap mbstring mysqli \
        opcache pcntl pdo \
        pdo_mysql pdo_pgsql pdo_sqlite \
        pgsql phar posix shmop simplexml \
        soap sockets sodium \
        sysvmsg sysvsem sysvshm tidy xml \
        xsl zend_test zip

# Установливаем Pecl расширения
RUN pecl install pcov \
    && docker-php-ext-enable pcov
RUN pecl install msgpack \
    && docker-php-ext-enable msgpack
RUN pecl install redis \
    && docker-php-ext-enable redis
RUN pecl install memcached \
    && docker-php-ext-enable memcached
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Устанавливаем Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
    && chmod 755 /usr/bin/composer

# Устанавливаем NodeJS
RUN curl -sL https://deb.nodesource.com/setup_$NODE_VERSION.x | bash -
RUN apt-get install -y nodejs
RUN npm install -g npm

# Устанавливаем Yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list
RUN apt-get update
RUN apt-get install -y yarn

# Вычищаем пакеты apt
RUN apt-get -y autoremove
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
