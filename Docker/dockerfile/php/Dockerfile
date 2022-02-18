FROM php:7.4-apache

# Set working directory
WORKDIR /var/www/html

# Install packages
RUN apt-get update -y && apt-get install -y --no-install-recommends \
    bash \
    nano \
    openssl \
    libzip-dev \
    libpng-dev \
    libxpm-dev \
    libfreetype6-dev \
    libmcrypt-dev \
    libssl-dev \
    libmcrypt-dev \
    libmemcached-dev \
    libwebp-dev \
    libjpeg62-turbo-dev \
    zlib1g-dev \
    libxml2-dev \
    libbz2-dev \
    unzip \
    zip \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/*
    

# Install php extensions
RUN docker-php-ext-install zip
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/# && \
    docker-php-ext-install gd
RUN docker-php-ext-install iconv mysqli pdo pdo_mysql bcmath json opcache intl xml soap -j$(nproc) gettext


# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV PATH="./vendor/bin:$PATH"


# Copy existing application directory contents
COPY ./src /var/www/html
# COPY ./nginx/default.conf /etc/nginx/conf.d

# Add UID '1000' to www-data
RUN usermod -u 1000 www-data

# Copy existing application directory permissions
COPY --chown=www-data:www-data ./src /var/www/html
COPY ./php/uploads.ini /usr/local/etc/php/conf.d/php.ini
#COPY . .
RUN chmod -R 775 /var/www/html/

# Change current user to www
USER www-data

#ENTRYPOINT ["/entrypoint.sh"]
# CMD ["apache2-foreground"]