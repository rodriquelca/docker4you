
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

# Switch to that user
# USER demo



# ARG PM_VERSION

# WORKDIR /tmp
# RUN wget https://github.com/ProcessMaker/processmaker/archive/refs/tags/v${PM_VERSION}.zip
# RUN unzip v${PM_VERSION}.zip && rm -rf /code/pm4 && mv processmaker-${PM_VERSION} /code/pm4

# WORKDIR /var/www/processmaker
# RUN composer install
# COPY build-files/laravel-echo-server.json /var/www/processmaker
# RUN npm install --unsafe-perm=true

# COPY build-files/laravel-echo-server.json .
# COPY build-files/init.sh .
# CMD bash init.sh && supervisord --nodaemon
