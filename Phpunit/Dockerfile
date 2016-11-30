# Very simple PHPUnit docker file
#

FROM php:7-apache

RUN set -xe; \
    groupadd -g 1032 alpine && \
    useradd -M -u 1031 -r -g alpine alpine


RUN printf "deb http://packages.dotdeb.org jessie all\ndeb-src http://packages.dotdeb.org jessie all\n" \
        >> /etc/apt/sources.list.d/extras.list

# Disable interactivity
ARG DEBIAN_FRONTEND=noninteractive

# Add the PHP7 repo
RUN curl https://www.dotdeb.org/dotdeb.gpg | apt-key add -

RUN apt-get update && apt-get install -y \
    git wget zip \
    libjpeg62-turbo-dev libmcrypt-dev libpng12-dev libicu-dev libxml2-dev \
    php7.0-xdebug  php7.0-soap php7.0-intl \
    php7.0-cli php7.0-apcu php7.0-gd php7.0-igbinary \
    php7.0-mcrypt php7.0-mongodb php7.0-redis php7.0-bcmath php7.0-xmlrpc

RUN for ext in gd bcmath xmlrpc intl pdo_mysql pcntl zip; \
        do docker-php-ext-install $ext > /dev/null; done

# Add composer
#RUN wget -t 30 -qO - http://getcomposer.org/installer | php \
#        && mv composer.phar /usr/local/bin/composer

RUN \
    wget -q http://getcomposer.org/download/1.2.2/composer.phar \
    && chmod +x composer.phar \
    && mv composer.phar /usr/local/bin/composer

USER alpine