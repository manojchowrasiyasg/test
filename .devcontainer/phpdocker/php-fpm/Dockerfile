FROM phpdockerio/php:8.2-fpm
WORKDIR "/workspace"

RUN apt-get update; \
    apt-get -y --no-install-recommends install \
        php8.2-bcmath \ 
        php8.2-gd \ 
        php8.2-gmagick \ 
        php8.2-mcrypt \ 
        php8.2-mysql \ 
        php8.2-pgsql \ 
        php8.2-xdebug; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
