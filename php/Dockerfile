FROM php:7.2-fpm

# Set working directory
WORKDIR /var/www

RUN apt-get update && apt-get install -y pkg-config patch
ADD https://git.archlinux.org/svntogit/packages.git/plain/trunk/freetype.patch?h=packages/php /tmp/freetype.patch
RUN docker-php-source extract; \
  cd /usr/src/php; \
  patch -p1 -i /tmp/freetype.patch; \
  rm /tmp/freetype.patch

# Install System Dependencies
RUN apt-get update && apt-get install -y \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libjpeg-dev \
  libmcrypt-dev \
  libpng-dev \
  libxml2-dev \
  libxslt-dev \
  zlib1g-dev \
  mariadb-client \
  && docker-php-ext-install bcmath \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install -j$(nproc) gd \
  && docker-php-ext-install exif \
  && docker-php-ext-install intl \
  && docker-php-ext-install pdo_mysql \
  && docker-php-ext-install soap \
  && docker-php-ext-install xsl \
  && docker-php-ext-install zip \
  && rm -rf /var/lib/apt/lists/*

# Add host's user group
RUN usermod -u 1000 www-data

# Install composer
RUN curl -sS https://getcomposer.org/installer -o ~/composer-setup.php && php ~/composer-setup.php --install-dir=/usr/local/bin --filename=composer

# set environment variable
ENV PATH="~/.composer/vendor/bin:$PATH"

# Change current user to www-data
USER www-data

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
