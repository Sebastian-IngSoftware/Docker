# Build dependencies with Composer
FROM composer:2.6 AS composer

# Production PHP-FPM image
FROM php:8.3-fpm-bookworm

# Install system deps and PHP extensions needed by the app
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        procps \
        git \
        unzip \
        curl \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        libwebp-dev \
        libxpm-dev \
        libxml2-dev \
        libonig-dev \
        libzip-dev \
        libsqlite3-dev \
        libssl-dev \
        libicu-dev \
        libxslt1-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp --with-xpm \
    && docker-php-ext-install gd intl mbstring pdo_mysql pdo_sqlite xsl zip mysqli exif pcntl ftp \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Copy Composer from the composer stage
COPY --from=composer /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# Copy application source
COPY . /var/www/html

ARG UID=1000
ARG GID=1000

RUN groupadd -g $GID appuser && useradd -u $UID -g $GID -m appuser

RUN find /var/www/html -type d -exec chmod 775 {} \; \
	&& find /var/www/html -type f -exec chmod 664 {} \; \
	&& chown -R appuser:appuser /var/www/html

USER appuser
