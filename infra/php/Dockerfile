FROM php:7.4-apache

# php ini
COPY ./php.ini /usr/local/etc/php/
# apache
COPY ./apache/apache2.conf /etc/apache2/apache2.conf
COPY ./apache/sites-available /etc/apache2/sites-available
COPY ./apache/sites-enabled /etc/apache2/sites-enabled
COPY ./apache/ssl /etc/ssl

# composer
RUN cd /usr/bin && curl -s http://getcomposer.org/installer \n\
| php && ln -s /usr/bin/composer.phar /usr/bin/composer

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
    apt-transport-https \
    apt-utils \
    build-essential \
    curl \
    debconf-utils \
    gcc \
    git \
    vim \
    gnupg2 \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libpq-dev \
    libzip-dev \
    locales \
    ssl-cert \
    unzip \
    zlib1g-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && echo "en_US.UTF-8 UTF-8" >/etc/locale.gen \
    && locale-gen \
    && a2enmod rewrite ssl \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install -j$(nproc) zip gd mysqli pdo_mysql opcache intl pgsql pdo_pgsql \
    && docker-php-ext-enable mysqli

# mailhog
RUN curl -sSLO https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 \
    && chmod +x mhsendmail_linux_amd64 \
    && mv mhsendmail_linux_amd64 /usr/local/bin/mhsendmail
