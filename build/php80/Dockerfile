FROM php:8.0-fpm

MAINTAINER php@evozon.com

# Install dependencies
RUN apt-get update \
  && apt-get install -y \
    build-essential \
    curl \
    cron \
    gnupg \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libxpm-dev \
    libxslt1-dev \
    sendmail-bin \
    sendmail \
    default-mysql-client \
    libc-client-dev \
    libkrb5-dev \
    libcurl3-dev \
    wget \
    libbz2-dev \
    nano \
    git \
    vim \
    locate \
    openssl \
    openssh-client \
    wget \
    unzip \
    libonig-dev \
    libxml2 \
    libzip-dev \
    rsync \
    libmcrypt-dev

RUN pecl channel-update pecl.php.net
RUN pecl install mcrypt

RUN docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp

# Configure the imap library
RUN docker-php-ext-configure \
  imap --with-kerberos --with-imap-ssl

RUN docker-php-ext-install \
  gd \
  mbstring \
  pdo_mysql \
  xsl \
  bcmath \
  imap \
  curl \
  iconv \
  intl \
  ctype \
  simplexml \
  zip \
  soap \
  sockets

# Install the 3.0 version of xdebug
RUN pecl install -o -f xdebug-3.0.0 && docker-php-ext-enable xdebug

# Install composer
RUN curl -LO https://getcomposer.org/download/latest-stable/composer.phar | php -- && mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer

# Install deployer
RUN curl -LO https://deployer.org/deployer.phar && mv deployer.phar /usr/local/bin/dep && chmod +x /usr/local/bin/dep

ARG SYMFONY_CLI_VERSION
RUN wget https://github.com/symfony/cli/releases/download/v$SYMFONY_CLI_VERSION/symfony_linux_amd64.gz \
    && gzip -d symfony_linux_amd64.gz \
    && mv symfony_linux_amd64 symfony \
    && chmod +x symfony \
    && mv symfony /usr/local/bin/
USER root

# set proper user
RUN usermod -u 1000 www-data
RUN usermod -G 100 www-data
RUN chown -R www-data:www-data /var/www/

# configure npm && grunt installation
RUN mkdir -p /var/www/.npm
RUN mkdir -p /var/www/.config
RUN chown -R www-data:www-data /var/www/.npm
RUN chown -R www-data:www-data /var/www/.config
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash - \
  && apt-get install -y nodejs
RUN npm install -g grunt-cli yarn

# Update search database
RUN updatedb

RUN apt-get -y clean && apt-get -y autoclean && apt-get -y autoremove && \
  rm -rf /var/lib/apt/lists/* && rm -rf && rm -rf /var/lib/cache/* && rm -rf /var/lib/log/* && rm -rf /tmp/*

USER www-data
WORKDIR /var/www/html

CMD ["php-fpm", "-F"]