FROM php:8.0.9-fpm

RUN apt-get update && apt-get install -y

RUN apt-get update && apt-get install -y --no-install-recommends \
        acl \
        libcurl4-openssl-dev \
        procps \
        zip \
        unzip \
        wget \
        git \
        zlib1g-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev && \
    docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ && \        
    docker-php-ext-install \    
        gd \
        zip \
        intl \
        mysqli \
        pdo \
        pdo_mysql \
        xml \
        curl \
        mbstring \
        opcache

RUN pecl install xdebug \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable xdebug

# RUN usermod -u 1000 www-data

RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
RUN wget https://get.symfony.com/cli/installer -O - | bash && mv /root/.symfony/bin/symfony /usr/local/bin/symfony
COPY ./docker/xdebug/xdebug.ini $PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini
COPY ./docker/opcache/opcache.ini $PHP_INI_DIR/conf.d/ddocker-php-ext-opcache.ini
RUN git config --global user.email "you@example.com" \ 
    && git config --global user.name "Your Name"
WORKDIR /var/www/symfony
