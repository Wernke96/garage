FROM php:8.1-fpm

RUN apt-get update && apt-get install -y \
    mariadb-client \
    git \
    vim \
    openssl \
    zip \
    libzip-dev \
    curl \
    wget \
    gnupg \
    apt-transport-https \
    lsb-release \
    libicu-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libicu-dev \
    libmagickwand-dev \
    libghc-postgresql-libpq-dev \
    libbz2-dev \
    libfontconfig && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install calendar zip intl pdo_mysql bcmath opcache pdo
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-install gd
RUN docker-php-ext-install bz2

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

COPY ./config/php.ini /usr/local/etc/php/

WORKDIR /code

# Make sure we're installing what we think we're installing!
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer

RUN curl -sL https://deb.nodesource.com/setup_16.x | bash - \
    && apt-get install -y nodejs

RUN curl -o- -L https://yarnpkg.com/install.sh | bash

RUN sed -i -e "s/pm.max_children\s*=\s*[0-9]*/pm.max_children = 30/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i -e "s/pm.max_spare_servers\s*=\s*[0-9]*/pm.max_spare_servers = 10/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i -e "s/;pm.max_requests\s*=\s*[0-9]*/pm.max_requests = 500/g" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i -e "s/access.log/;access.log/g" /usr/local/etc/php-fpm.d/docker.conf

ENV PATH "$PATH:/root/.yarn/bin"

EXPOSE 9000
CMD ["php-fpm"]
