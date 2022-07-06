
FROM php:7.4-fpm-alpine

# Install extensions


RUN apk add --update --no-cache \
    autoconf g++ imagemagick imagemagick-dev libtool make pcre-dev \
    libzip-dev \
    zip \
    libpng \
    freetype-dev libpng-dev libjpeg-turbo-dev
    
    
RUN  apk add --update --no-cache docker openrc
#RUN rc-update add docker boot 

#
# docker client
#
ENV DOCKERVERSION=20.10.5
RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKERVERSION}.tgz \
  && tar xzvf docker-${DOCKERVERSION}.tgz --strip 1 \
                 -C /usr/local/bin docker/docker \
  && rm docker-${DOCKERVERSION}.tgz

RUN pecl install imagick \
 && docker-php-ext-enable imagick

RUN docker-php-ext-install gd pdo_mysql bcmath opcache pcntl exif zip



# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Configure PHP
COPY php.ini $PHP_INI_DIR/conf.d/opcache.ini

# Use the default development configuration
RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

# Create user based on provided user ID
ARG HOST_UID
RUN adduser --disabled-password --gecos "" --uid $HOST_UID demo



#
# node
#

RUN apk add --update nodejs npm

# Switch to that user
# USER demo