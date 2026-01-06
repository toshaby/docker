FROM php:8.4-apache

ARG NODE_VERSION=24

ENV APACHE_DOCUMENT_ROOT=/var/www/html/public

RUN sed -ri \
    -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' \
    /etc/apache2/sites-available/*.conf && \
    sed -ri \
    -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' \
    /etc/apache2/apache2.conf \
    /etc/apache2/conf-available/*.conf

# PHP extensions
RUN apt update && apt install -y \
    curl \
    unzip \
    libzip-dev \
    ca-certificates \
    gnupg \
    && curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - \
    && apt install -y nodejs \
    && docker-php-ext-install \
    pdo \
    pdo_mysql \
    zip \
    exif \
    && a2enmod rewrite \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# ===== Composer =====
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
