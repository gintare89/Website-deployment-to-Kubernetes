FROM node:16-alpine AS assets-build

WORKDIR /var/www/html

COPY package*.json ./

RUN npm install --legacy-peer-deps

COPY . .

RUN npm run dev

FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    git \
    curl \
    unzip \
    && docker-php-ext-install mysqli pdo pdo_mysql mbstring pcntl exif zip \
    && apt-get autoclean -y \
    && rm -rf /var/lib/apt/lists/*

COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer

WORKDIR /var/www/html

COPY . /var/www/html/

RUN rm -f bootstrap/cache/*.php

RUN composer install --no-scripts --no-dev --no-interaction --no-progress

COPY apache_config.conf /etc/apache2/sites-available/000-default.conf
COPY --from=assets-build /var/www/html/public /var/www/html/public

RUN chown -R www-data:www-data /var/www/html/storage
RUN chown -R www-data:www-data /var/www/html/bootstrap
RUN chmod -R 775 /var/www/html/storage
RUN chmod -R 775 /var/www/html/bootstrap


RUN a2enmod rewrite

EXPOSE 80

CMD ["apache2-foreground"]
