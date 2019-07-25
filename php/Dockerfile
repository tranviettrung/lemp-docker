FROM php:7.2-fpm

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
  build-essential \
  libpng-dev \
  libjpeg62-turbo-dev \
  libfreetype6-dev \
  libmcrypt-dev \
  libxml2-dev \
  locales \
  zip \
  jpegoptim optipng pngquant gifsicle \
  vim \
  unzip \
  git \
  curl

# Add host's user group
RUN usermod -u 1000 www-data

# Install extensions
RUN docker-php-ext-install bcmath pdo_mysql zip exif pcntl

# Install composer
RUN curl -sS https://getcomposer.org/installer -o ~/composer-setup.php && php ~/composer-setup.php --install-dir=/usr/local/bin --filename=composer

# set environment variable
ENV PATH="~/.composer/vendor/bin:$PATH"

# Change current user to www-data
USER www-data

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
