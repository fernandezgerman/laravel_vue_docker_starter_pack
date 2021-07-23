FROM php:8.0-fpm

# Copy composer.lock and composer.json
COPY composer.lock composer.json /var/www/

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl

#X DEBUG
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug


#Node
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get install -y nodejs


# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*


# Install extensions
RUN apt-get update && apt-get install -y curl libmcrypt-dev openssh-client zlib1g-dev ssh unzip cron libicu-dev vim g++ gcc libpcre3-dev make git pkgconf re2c zlib1g-dev wget mc libzip-dev libgmp-dev ffmpeg libmagickwand-dev --no-install-recommends
RUN docker-php-ext-install pdo_mysql zip exif pcntl
RUN docker-php-ext-install gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY . /var/www
ADD ./docker/php.ini /usr/local/etc/php/conf.d/php.ini
ADD ./docker/crontab /etc/crontab

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
