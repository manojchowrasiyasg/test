FROM alpine:3.17

ARG COMPOSER_HASH=55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae
ARG DRUSH_VERSION=8.1.18

LABEL org.label-schema.schema-version="1.0" \
  org.label-schema.name="docker-php" \
  org.label-schema.description="PHP Alpine for Drupal - composer & drush" \
  maintainer="Alexey Gaydabura <gaydabura@gmail.com>"

ENV PHPRUN_DEPS \
  curl \
  git \
  make \
  mariadb-client \
  openssh-client \
  patch \
  sqlite

RUN apk add --no-cache \
  php81 \
  php81-pecl-apcu \
  php81-pecl-igbinary \
  php81-bcmath \
  php81-ctype \
  php81-curl \
  php81-dom \
  php81-gd \
  php81-gmp \
  php81-iconv \
  php81-json \
  php81-mbstring \
  php81-opcache \
  php81-openssl \
  php81-pcntl \
  php81-pdo_mysql \
  php81-pdo_sqlite \
  php81-phar \
  php81-session \
  php81-simplexml \
  php81-tokenizer \
  php81-pecl-xdebug \
  php81-xml \
  php81-xmlreader \
  php81-xmlwriter \
  php81-xmlwriter \
  php81-fileinfo \
  php81-zip \
  php81-pecl-memcached \
  #php81-imagick \
  $PHPRUN_DEPS \
  && ln -fs php81 /usr/bin/php \
 && curl -sL -o /usr/bin/drush https://github.com/drush-ops/drush/releases/download/8.1.18/drush.phar \
 && chmod +x /usr/bin/drush

# Install composer.
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php -r "if (hash_file('SHA384', 'composer-setup.php') === getenv('COMPOSER_HASH')) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php \
  --install-dir=/usr/bin \
  --filename=composer \
  && php -r "unlink('composer-setup.php');"


RUN set -e \
  && mkdir -p /workspace \
  && addgroup -g 1000 -S www-skilld \
  && adduser -u 1000 -D -S -G www-skilld www-data \
  && apk add --no-cache \
  php81-fpm

RUN apk add --no-cache --repository http://dl-3.alpinelinux.org/alpine/edge/testing gnu-libiconv
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so

COPY php.ini /etc/php81/conf.d/xx-drupal.ini
COPY php-fpm.conf /etc/php81/

#USER www-data
WORKDIR /workspace
VOLUME /workspace

EXPOSE 9000

CMD ["php-fpm81", "-F"]

