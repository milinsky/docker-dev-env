FROM php:fpm

# Update packages
RUN apt-get update

# Install packages
RUN apt-get -y install curl unzip npm

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
    && chmod 755 /usr/bin/composer

# Install additional PHP libraries
RUN apt-get install -y libpq-dev libonig-dev libxml2-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pgsql pdo_pgsql bcmath mbstring tokenizer xml intl

WORKDIR /src
